# Different Configuration App
One of the best features I enjoyed using is setting up multiple environments for my projects. Most applications will probably use at the very least two environments: **Production** and **Development**. Most likely, larger applications will be running several environments, such as **QA** , **Staging** and so on.

# Scenario 
By default the Angular CLI bootstraps a new project with two files within the folder **environments**: 
* environments.ts 
* environments.prod.ts

In this demo I will add another new environment file named
* environments.staging.ts

# Steps
1. Create Staging environment file

From the source of your repo run Git-Bash and type  :
```
$ cd src/environments
$ touch environment.staging.ts
```
This will create a new **environment.staging.ts** inside environments folder

2. Change **build** section in **angular.json**

At the root of your application you will find angular.json file please open it in your favorite editor and locate the **configuration** section under **build** section
```
...
  "build": {
         ...
          "configurations": {
            "production": {
              "fileReplacements": [
                {
                  "replace": "src/environments/environment.ts",
                  "with": "src/environments/environment.prod.ts"
                }
              ],
             ...
            },
            "ci": {
              "progress": false
            }
          }
        }
```

Here you will find **fileReplacements** section that is responsible to tell the angular cli that the development environment file **environment.ts** will be replaced with **environment.prod.ts** when run the following command
``` 
$ ng build --prod
```
which also is equivalent to command
``` 
$ ng build --configuration=production
```
so to add our new staging environment file we need to change **angular.json** to be as following
```
...
  "build": {
         ...
          "configurations": {
            "production": {
              "fileReplacements": [
                {
                  "replace": "src/environments/environment.ts",
                  "with": "src/environments/environment.prod.ts"
                }
              ],
             ...
            },
            "staging":{
              "fileReplacements": [
                {
                  "replace": "src/environments/environment.ts",
                  "with": "src/environments/environment.staging.ts"
                }
              ]
            },
            "ci": {
              "progress": false
            }
          }
        }
```
and now we can run the following command
```
$ ng build --configuration=staging
```
3. Change **serve** section in **angular.json**

Try to locate the following section in **angular.json**

```
 "serve": {
       ...
          "configurations": {
            "production": {
              "browserTarget": "app:build:production"
            },
           "ci": {
              "progress": false
            }
          }
        },
```

then add the new staging section for your new file as following
```
"serve": {
         ...
          "configurations": {
            "production": {
              "browserTarget": "app:build:production"
            },
            "staging": {
              "browserTarget": "app:build:staging"
            },
            "ci": {
              "progress": false
            }
          }
        },
```

now if you run the following command
```
$ ng serve --configuration=staging
```
this will replace the default development environment file with environment.staging.ts file

# How to taste it
* Clone repo on your machine
* Run the development mode using following command
```
$ ng serve -o
```
* Check welcome message. You will see
> <h4>Welcome From Development </h4>
* Stop the application 
* Run the staging mode using following command

```
$ ng serve --configuration=staging -o
```
* Check welcome message again.You will see
> <h4>Welcome From Staging </h4>
* Stop the application 
* Run the production mode using following command

```
$ ng serve --configuration=production -o
```
* Check welcome message again.You will see
> <h4>Welcome From Production </h4>

# Takeaways :point_down:
* Setting up multi-environments with the Angular CLI is pretty easy and powerful, add as many as you need
* Adding configuration objects at build-time is powerful, but don’t add sensitive information
