# mini-birds-mongoose (Part I)

_Goal_: Build a **bird sighting** API in Express using four common CRUD operations in MongoDB using MongoJS.

## Sample Data

Note the following, expanded sample data for bird sightings.

```json
[{
  "name": "red breasted merganser",
  "order": "Anseriformes",
  "status": "least concern",
  "confirmed": true,
  "numberSeen": 2
},
{
  "name": "cedar waxwing",
  "order": "Passeriformes",
  "status": "least concern",
  "confirmed": false,
  "numberSeen": 5
},
{
  "name": "osprey",
  "order": "Accipitriformes",
  "status": "least threatened",
  "confirmed": true,
  "numberSeen": 2
},
{
  "name": "snowy plover",
  "order": "Charadriformes",
  "status": "extinct",
  "confirmed": true,
  "numberSeen": 7
}]
```

## Step 1: Clone the repo

Create your Express.js app by cloning this repository.

## Step 2: Install the NPM modules

```
npm install
```

## Step 3: Test it

Start your application by running:

```
node server.js
```

Test each of your endpoints in Postman with the following URLs:

* **POST** `/api/sighting`
* **GET** `/api/sighting?region=america`
* **PUT** `/api/sighting?id=some-id`
* **DELETE** `/api/sighting?id=some-id`

If everything's working, you'll see console output each time you hit your endpoints.

## Step 4: Start mongod

Start the mongo daemon in a separate terminal window.

## Step 5: Require and connect to Mongo

Now, require the Mongoose module, and create a database by connecting to it in `server.js`. Name your database `birds-mongoose`.

HINT: [Read the documentation](http://mongoosejs.com/docs/index.html)

## Step 6: Create a _Sighting_ schema and model

In a separate file called, 'Sighting.js', create a schema for the Sighting collection using the sample data above.

Your schema should:

* Give each property a data type.
* Make `name` lowercase, and required.
* Restrict the length of `order` to 20.
* Enumerate possible values for `status`; make them lowercase.
* Ensure `numberSeen` is greater than 0.
* Set `confirmed` to `false` by default.

Now, define a model for your schema, and add it to the file's exports.

Finally, declare a var for your Sighting model in `server.js`.

## Step 7: Upgrade 'POST' endpoint to record a sighting

Upgrade your POST endpoint with code to create a sighting document from the `body` of the request, using your Sighting model.

Use sample data from `sightings.json` in your request body.

HINT: [Read the documentation for saving with models.](http://mongoosejs.com/docs/models.html)

For steps 7 through 10, test each of your endpoints again.

## Step 8: Upgrade 'GET' endpoint to retrieve a sighting

Modify the GET endpoint to retrieve all sightings with a given `status`, as stated in the request query.

## Step 9: Upgrade 'PUT' endpoint to modify a sighting's order

Update your PUT endpoint to accept a `body` modifying an existing sighting's `order` field. Use the `id` parameter in the query string to identify the sighting to change.

Return the newly updated sighting in your response.

## Step 10: Upgrade 'DELETE' endpoint to delete a sighting

Update your DELETE endpoint to delete a sighting document by `id` in the query string.


## Copyright

© DevMountain LLC, 2016. Unauthorized use and/or duplication of this material without express and written permission from DevMountain, LLC is strictly prohibited. Excerpts and links may be used, provided that full and clear credit is given to DevMountain with appropriate and specific direction to the original content.

# mini-birds-mongoose (Part II)

Expand on yesterday's project to support Birds and Users.

## Objectives

* Practice embedded relationships.
* Practice reference relationships.

We will:

* Add a User model.
* Add a Bird schema.
* Refactor our Sighting model to support User via reference, and Bird via an embedded document.
* Add a POST endpoint for User.
* Adjust our GET endpoint for the Sighting model.

## Prep Step: Change db name

To avoid data collisions, let's change the db name to `birds-mongoose-2`.

## Step 1: Understanding our Data Structure

Identify the type of relationships we will be creating.

### Sightings

```json
[{
  "user": "aUserID",
  "birds": [{
    "name": "red breasted merganser",
    "order": "Anseriformes",
    "status": "least concern"
  },
  {
    "name": "cedar waxwing",
    "order": "Passeriformes",
    "status": "least concern"
  }],
  "confirmed": true,
  "numberSeen": 2
},
{
  "user": "aUserID",
  "birds": [{
    "name": "osprey",
    "order": "Accipitriformes",
    "status": "least concern"
  }],
  "confirmed": false,
  "numberSeen": 1
}]
```

### Users

```json
[{
  "email": "doh@msn.com",
  "username": "homersimpson2000",
  "level": 98,
  "location": "Springfield",
  "member": true
},
{
  "email": "da.swamp@crocs.com",
  "username": "ridnick1",
  "level": 2,
  "location": "Louisiana",
  "member": false
}]
```

## Step 2: Create the _Bird_ schema to be embedded into the _Sighting_ model

In a new file, `Bird.js`, create a Bird schema using properties from the existing Sighting model. Name, order, and status will be the properties moved to our Bird object.

## Step 3: Create the _User_ Model

In a new file, `User.js`, create a User model with the schema properties email, username, level, location, and member.

Declare a var referencing your _User_ model in `server.js`.

## Step 4: Refactor our _Sighting_ Model

Add a property to the Sighting schema called `user` that will create a relationship between a _User_ and and _Sighting_. Each _Sighting_ should be required to be related to only one _User_. A user may have multiple sightings.

Add another property called `bird` that will store embedded data related to a specific bird when a new sighting is created.

## Step 4: Add POST for _User_

Add a POST endpoint for creating new users: `/api/users`. Test it with real data.

## Step 5: Refactor Sighting Endpoints

* POST a new `/api/sighting`, this time using the new _Sighting_ data and a _User_ id.
* When GET `/api/sighting` is requested make sure to populate it with _User_ data before returning it to the client.
* Make it possible for the client to request sightings for a specific user by through sending the user id as a part of the request query in addition to `status`.

## Copyright

© DevMountain LLC, 2016. Unauthorized use and/or duplication of this material without express and written permission from DevMountain, LLC is strictly prohibited. Excerpts and links may be used, provided that full and clear credit is given to DevMountain with appropriate and specific direction to the original content.
