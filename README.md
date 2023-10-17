First React laravel application

1. Create Project
   
   In laravel if we building and api application then we use following options during project creation
   
   laravel create rest-api
   
   breeze
   
   api
   
   mysql
   
   Breeze command removes blade template and adds auth.php in route folder to manage user login and registration.

2. Database
   
   Data base used in project is called rest_api.
   
   It has table name skills. Table has five columns
   
   id,name,slug,created_at,updated_at
   
   id field auto increments. created_at and updated_at have datatype times timestamp and values added automatically at time of insertion and
   updation.
   
4. Routes
   
   All routes are defined in api.php file.
   
   API application has version numbers which are updated from time to time. To access resource url should include domain_name/api/versionNo
   
   All routes are group by prefix v1 and then pass through apiResource function.
   
   In apiResource controller SkillController and url /skills is used.
   
   apiResource uses build in functions in skillController based on request method and url.
   
   If request method is get and url is skills then it calles index() function
   
   If request method is get and url has parameter ie /skills/id-number then it calls show(Skill $request) function
   
   Where Skill is model name corresponding to skills table.
   
   If request method is post and url is skills then it calls store(StoreSkillRequest $request)
   
   Where StoreSkillRequest is used in authorization, validation and property merging and its variable contains json data.
   
   If request method is put and url is /skills/id-number then it calls update(StoreSkillRequest $request,Skill $skill)
   
   Where json data is stored in $request and id is stored in $skill.
   
   If request method is patch and url is /skills/id-number then it calls update(StoreSkillRequest $request,Skill $skill)
   
   Where json data is stored in $request and id is stored in $skill.
   
   Patch is used if some of the columns of record are to be updated.
   
   If request method is delete and url is /skills then delete(Skill $skill) funtion is called.

6. Model
   
   To create a model we use command
   
   php atisan make:model Skill --all
   
   Where all command creates Skill Model, StoreSkillRequest,SleepFactory,SleepSeeder, migration file, SkillController and SleepPolicy.
   
   Some of these files are explained in next section.
   
   In Skill model file if we donot want to use timestamp then we can use variable $timestamp = false.
   
   Similarly it as array $fillable which contain column names than can be updated or records added to.
   
   Suppose we have Skill object $skill, then $skill->create() and $skill->update() will only effect columns in $fillable array

8. Migration, factory, Seeding
    
   Once migration, factory and seeding files are created we can use then to create table skills and populate it using SkillFactory and
   
   SkillSeeder file. In SkillFactory we define fake data types for columns with properties such as unique or selection crietaria such as
   
   inbetween, less then etc.
   
   In SkillSeeder we define number of rows to of table to be populated using factory's fake data.
   
   To execute all of them we use command: php artisan migrate:fresh --seed

10. Request/Response/Controller
    
   When browser send a post,put,patch request it first StoreSkillRequest file for authrisation check then validation check. If user is not
   authorise in case of token requirement then error message is returned. Then data is checked for validation in rules() function. If there is       matching error then error message with status code 422 is sent to client.
   
   Then validated data is stored using create or update command based on request.
   
   If client request record details against id then request is stored in Skill object $skill. It stores all record information related to id.
   We then pass it to response object where we define columns names and their values that are to be return to client. We can change column
   name with respect to column name in database table and we can exclude column name, if we donot want client to access it ie password column
   or timestamp column etc.
   
   If request is to list skills then in index() function. We can return
   
   Skill::all() or   Skill::->paginate() but these functions return all columns. To restrict columns we use
   
   SkillResource::collection(Skill->pagination()) or SkillCollection(Skill->paginate()). SkillCollection use SkillResource for all records and
   return only fields in SkillResource
   
   Search is not part of apiResource. If we want to search name with wildcard %a% we have to create our own function for route
   
   url: https://localhost:8000/api/v1/search/a
   
   Route::get('/search/{name}',[SkillController::class,'search'])
   
   In controller's search function we use where clause and use like
   
   
   
