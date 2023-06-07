

# Setup Environment
Install XAMPP

Install Composer (package manager for PHP)

1) Go to <https://getcomposer.org/download/>
2) Install composer( [Composer-Setup.exe](https://getcomposer.org/Composer-Setup.exe "Click here to download Composer-Setup installer for Windows") )
3) Open hyper terminal and run following command
```
composer global require laravel/installer
```
4) Create a new application by following command (first\_app=>app name)
```
laravel new first_app
```
5) Go to project folder
```
cd .\first\_app\
```
6) Open this folder in VS-Code
7) Run the project by following command
```
php artisan serve
```
8) Go to chrome and type

http://localhost:8000/


# Routing
In Laravel, routes allow users to route the appropriate request to the desired controller. The most basic Laravel Route accepts a Uniform Asset Identifier (your route path) and a closure which can be both a function or a class.

In Laravel, routes are created inside the web.php and api.php files. Laravel comes with two routes by default: one for the WEB and one for the API.

![](https://github.com/muhammadnaqeeb/Guide-to-Laravel/blob/main/Images/001.png)
```
Route::get('/greeting', function () {
     return 'Hello World';
})
```
Initial route
```
Route::get('/', function () {
    return "Hello, World!";
});
```
You can also return html
```
Route::get('/', function () {
    return "<h1>Hello, World!</h1>";
});
```
Making about route
```
Route::get("/about", function(){
    return "<h1>About</h1>";
});
```
In chrome

http://localhost:8000/about


# Display View
- Make file in
```
resources -> views -> yourFile.blade.php
```
- Then make a route
```
routes -> web.php
```

```
Route::get("/about", function(){
      return "<h1>About</h1>";
});
```
# Place and link CSS file
Make css folder inside public folder and make style.css file there.

Link file:
```
<link rel="stylesheet" href="{{ asset('css/style.css') }}">
```
# Add image
Create an images folder inside public folder

Add image:
```
<img id="myImageId" src="{{ asset('images/menu.png') }}"></img>
```

# Add link in “a” tag
In route-> web.php
```
Route::get('/signup', function () {
      return view('Signup');
});
```
In view
```
<li><a  href="{{url('/signup')}}">Sign Up</a></li>
```
# View using controller (recommended)
Firstly, make controller use following command in terminal
```
php artisan make:controller NameOfController
```
e.g,
```
php artisan make:controller LoginController
```
![](https://github.com/muhammadnaqeeb/Guide-to-Laravel/blob/main/Images/002.png)

Then make function in controller class (controller which you have created) which returns a view
```
<?php
namespace App\Http\Controllers;
use Illuminate\Http\Request;
class LoginController extends Controller
{
      public function Loginn(){ // <-- this function here
        return view('Login');
    }
}
```
Then go to routes -> web.php

import
```
use App\Http\Controllers\LoginController;
```
```
Route::get('/login', [LoginController::class,'Loginn'])->name('login.Loginn');
```
Or without ->name()
```
Route::get('/login', [LoginController::class,'Loginn']);
```
General form
```
Route::get('/login', [controllerName::class,'methodInController'])->name('login.methodInController’);
```
# Passing data from Controller to View
### **Passing Simple String**
In controller class function which is returning a View(view which we have to send data)
```
class LoginController extends Controller
{
    public function Loginn(){
        $mainHeadingText = 'Welcome Back'; // data to send
        return view('Login')->with('mainHeadingText',$mainHeadingText);
    }
}
```
In View
```
<h1>{!! $mainHeadingText !!}</h1>
```
### **Passing an array**
```
class LoginController extends Controller
{
    public function Loginn(){
        //$mainHeadingText = 'Welcome Back'; // data to send
        $arrayToPass = ['a','b'];
        return view('Login')->with('arrayToPass',$arrayToPass);
    }
}
```
In View
```
<h1>{!! $arrayToPass[0] !!}</h1>
```
You can also use loop here


# Database in Laravel
Create View and controller and define route

- **Go to [http://localhost/phpmyadmin/](http://localhost/phpmyadmin/)**
- **Create a database in phpmyadmin**

![](https://github.com/muhammadnaqeeb/Guide-to-Laravel/blob/main/Images/003.png)

- **Go to .env file and add your database name there**

![](https://github.com/muhammadnaqeeb/Guide-to-Laravel/blob/main/Images/004.png)

- **Run following commands in terminal**
```
php artisan config:cache
```
```
php artisan migrate
```
This will create some table automatically in your database

- **Create a table using following command**
```
php artisan make:migration createJoblistTable
```
This will create a php file inside database/migrations directory name like (data_create_joblist_table).

![](https://github.com/muhammadnaqeeb/Guide-to-Laravel/blob/main/Images/005.png)

- **Specify columns(attributes) of table in up() function in the php file created in previous step**
```
return new class extends Migration
{
    /**
     * Run the migrations.
     */
    public function up(): void
    {
        Schema::create('joblist', function (Blueprint $table) {
            $table->id('job_id');
            $table->string('job_title',100);
            $table->string('job_type',100);
            $table->string('job_description',1000);
            $table->string('job_location',100);
            $table->string('job_salary',100);
            $table->string('job_application_deadline',100);
            $table->string('job_contact',100);
            $table->timestamps();
        });
    }
```
Class and up() function is pre-created you only need to add attributes

- **Then run following command again**
```
php artisan migrate
```
This will send/migrate this table to phpmyadmin

![](https://github.com/muhammadnaqeeb/Guide-to-Laravel/blob/main/Images/006.png)

It will remove **date** and **create** from start and **table** from end of file name and remaining will be table name in database

- **Insert one row manually** 
```
Insert -> (add data by your self) -> Go
```
## **Display/Read (CRUD)**
- **Make Model using following command**
```
php artisan make:model joblistModel
```
In model class add following lines
```
class joblistModel extends Model
{
    use HasFactory;
    // these lines
    protected $table='joblist'; // joblist is table name
    private $primary = 'job_id';
}
```
- **Go to controller file and add another function**
```
use App\Models\joblistModel;
   public function DisplayJobList(){
        $joblistmodel = joblistModel::all();
        print_r($joblistmodel);
    }
```
- **Go to web,php and define route**
```
Route::get('/displayJobs', [JobListController::class,'DisplayJobList']);
```
When you visit <http://127.0.0.1:8000/displayJobs> You will see all object in array on screen.

- **Now pass this data to view**
```
In job controller file
    public function DisplayJobList(){
        $joblistmodel = joblistModel::all();
        return view('jobList')->with('jobListData', $joblistmodel);
    }
```
- **Check data on view**
```
<body>
    {{$jobListData}}
```
Displaying data using loop in view
```
@foreach($jobListData as $job)
   <div class="tab-content">
       <div id="tab-1" class="tab-pane">
           <div class="job-item p-4 mb-4">
               <div class="row g-4">
                  <div class="col-sm-12 ">
                      <div class="text-start ps-4">
                         <h5 class="mb-3">{{$job->job_title}}</h5>
               </div>
             </div>
           </div>
          </div>
         </div>
      </div>
@endforeach
```
Simplified
```
@foreach($jobListData as $job)
   <div class="text-start ps-4">
      <h5 class="mb-3">{{$job->job_title}}</h5>                      
   </div>
@endforeach
```
## **Create/Insert (CRUD)**
- **Create a new view for adding data**
- **Add function in jobListController**
```
    public function AddJobb(){
        return view('AddJob');
    }
```
- **Add route in web.php file**
```
Route::get('/addJob', [JobListController::class,'AddJobb']);
```
- **Add form in view**
```
       <form action="{{url('/')}}/jobAddedSuccessfully" method="post">
        <!-- this is security check -->
        @csrf
        <label for="job_title">Job Title</label>
        <input type="text" Name="jobTitle" id="jobTitle" />
        <label for="job_location">Job Location</label>
        <input type="text" Name="jobLocation" id="jobLocation" />
        <button >Submit</button>
    </form>
```

- **Go to web.php file and register a route**
```
Route::post('/jobAddedSuccessfully', [JobListController::class,'jobAddedSuccessfullyy']);
```
In this route we write logic for adding data into database

- **In job controller add jobAddedSuccessfullyy function in which we add logic to add data to table**
```
    public function jobAddedSuccessfullyy(Request $req){
        $job = new joblistModel();
        $job->job_title = $req['jobTitle']
        $job->job_location = $req['jobLocation'];
        $job->save()
        echo "Job Added Successfully"
    
```

