# Hack101 - Android: Lesson 3 #


What is Parse?
--------------------

In Lesson 3, we built a SQL database for our to-do app and used it to store our tasks. In this tutorial, you'll see how easy it is to swap that out for a Parse database. 

Parse is, to put it very simply, an online database for your app. By keeping your data centralized online, you can easily build cross-platform apps that share the same database.Unlike SQL, Parse is a No-SQL database (built on top of Mongo). Instead of tables, we have collections: those are essentially tables except that they do not enforce a schema. Objects in collections are just like rows in tables, but with added flexibility. 

Step 1: install Parse 
---------------------------------------------

First, go to https://www.parse.com and set up an app, then follow the instructions at https://www.parse.com/apps/quickstart#parse_data/mobile/android/native/existing to set up the Parse API
 

Step 2: Set up your database
---------------------------------------------

Initializing your db requires your API key and secret:

```java
    // Enable Local Datastore.
    Parse.enableLocalDatastore(this);
    Parse.initialize(this, <api key here>, <api secret here>);
```
The Parse local datastore is an offline database maintained in your app, so that the app can still function if it is not connected to the internet. This allows you to synchronize with the online db whenever it is most convenient.

First, let's push some to-do items to Parse:

```java
    ParseObject todoObject = new ParseObject("TodoObject");
    todoObject.put("todo", todoItem);
    todoObject.saveInBackground();
```

The ParseObject above is the class defining any objects we put, read or manipulate from the Parse database. We just pushed our first item and created the collection (or "table") in a few lines of code! 

In Parse, you can create a new Collection by simply pushing a new object to it. In our case, we can create the `ToDoObject` collection by pushing a new to-do item from our Android app. Any fields that did not previously exist in the collection will be created on the fly! In our case, the only field is `todo`. But we can add an extra field (such as `author`) to one of our objects, without changing any other entries.


Now, let's see how we can fetch all those items from the Parse database online:

````
        ArrayList<String> todoListItems = new ArrayList<String>();
        ParseQuery<ParseObject> query = ParseQuery.getQuery("TodoObject");
        ArrayList<ParseObject> todoList = new ArrayList<ParseObject>();
        try {
            todoList = (ArrayList) query.find();
        } catch (ParseException e) {
            e.printStackTrace();
        }
        for (ParseObject todoItem : todoList) {
            String task = todoItem.getString("todo");
            todoListItems.add(task);
        }
````

Step 2: Code! 
---------------------------------------------

Now it's your turn to try! Add the code that will remove a to-do item from the database when you click on it. 

Check out the documentation on queries for help: https://www.parse.com/docs/android_guide#queries