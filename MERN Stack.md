# MERN Stack

> This is my cheatsheet for MERN stack development

## Express GraphQL Server

#

### Create Development Environment

Initiate **npm**

```
npm init
```

Configure the following values

```
package name: {APP NAME}
description: {APP DESCRIPTION}
entry point: {server.js}
```

Install dependencies

```
npm i graphql express-graphql express axios
npm i -D nodemon
```

Configure package.json

```
"scripts": {
    "start": "node server.js"
    "server": "nodemon server.js"
}
```

Create server.js and add following values:

```
const express = require('express');
const graphqlHTTP = require('express-graphql');
const schema = require('./schema.js')

const app = express();

app.use(
    '/graphql',
    'graphqlHTTP({
        schema,
        graphiql: true
    })
);

const PORT = process.env.PORT || 5000;
app.listen(PORT, () => console.log(`Server started on port ${PORT}`));

```

## Create GraphQL Schema

Analyze REST, GraphQL or MongoDB API.
Example case: https://docs.spacexdata.com/

Launches API data

https://docs.spacexdata.com/#bc65ba60-decf-4289-bb04-4ca9df01b9c1

Create **schema.js**

Add item specific query

```
const {
    GraphQLObjectType,
    GraphQLInt,
    GraphQLString,
    GraphQLBoolean,
    GraphQLList
    GrapgQLSchema
} = require(graphql');

// Launch type
const LaunchType = new GraphQLObjectType({
    name: 'Launch'
    fields: () => ({
        flight_number: ( type: GraphQLInt ),
        mission_name: ( type: GraphQLString ),
        launch_year: ( type: GraphQLString ),
        launch_date_local: ( type: GraphQLString ),
        launch_success: ( type: GraphQLBoolean ),
        rocket:{ type: RocketType }
    })
});
```

Add object-specific Query:

**RocketType**

```
// Rocket Type
const RocketType = new GraphQLObjectType({
    name: 'Rocket'
    fields: () => ({
        rocket_id: ( type: GraphQLInt ),
        rocket_name: ( type: GraphQLString ),
        rocket_type: ( type: GraphQLString )
    })
});
```

Add a Root Query

```
const axios = require('axios');
```

```
const RootQuery = newGraphQLObjectType({
    name: 'RootQueryType',
    fields: {
        launches: {
            type: new GraphQLList(LaunchType),
            resolve(parent,args) {
                return axios
                .get('https://api.spacexdata.com/v3/launches')
                .then(res.data)
            }
        launch: {
            type: LaunchType,
            args: {
                flight_number: {type: GraphQLInt}
            },
            resolve(parent, args) {
                return axios
                .get(`https://api.spacexdata.com/v3/launches/${args.flight_number`})
                .then(res => res.data);
            }
        rocket: {
            type: RocketType,
            args: {
                id: {type: GraphQLInt }
            },
            resolve(parent, args) {
                return axios
                    .get(`https://api.spacexdata.com/v3/rockets/${args.id}`)
            }
        }
        }
    }
});

module.exports = new GraphQLSchema({
    query: RootQuery
});
```

Run server in terminal

```
npm run server
```

And open graphiQL

```
localhost:5000/graphql
```

Test the schema!

##
