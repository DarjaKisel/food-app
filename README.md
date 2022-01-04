# Food App

The Understand GraphQL for Beginners' repository to learn how to create GraphQL objects and queries.

## Installation instruction

```bash
# Install gems, make sure the ruby version is included in `chruby`
$ bundle 

# Migration and seeds
$ bundle exec rake db:create db:migrate db:seed

# Run app
$ bundle exec rails s -p3000
```
Blog series:
- https://shopify.engineering/understanding-graphql-beginner-part-one
- https://shopify.engineering/understanding-graphql-beginner-part-two
- https://shopify.engineering/understanding-graphql-for-beginners-part-three

## Run example queries
Navigate to [http://localhost:3000/graphiql](http://localhost:3000/graphiql)

### Read

1. Get all foods with: id, placeOfOrigin
```bash
query {
  allFood {
    image
    placeOfOrigin
  }
}
```
2. Get all nutrition facts with: id, foodId, calories, protein, totalFat
```bash
query {
  nutrition {
    id
    foodId
    calories
    protein
    totalFat
  }
}
```
3. Get all foods with their: name, nutrition facts with: servingSize, calories
```bash
query {
  allFood {
    name
    nutrition {
      servingSize
      calories
    }
  }
}
```
4. Get food by id with its: name, nutrition facts with: servingSize, calories
```bash
query {
  food(id: 1) {
    name
    nutrition {
      servingSize
      calories
    }
  }
}
```
5. Find food by an origin with its: name, nutrition facts with: servingSize, calories
```bash
query {
  findFood(placeOfOrigin: "China") {
    id
    name
    nutrition {
      calories
    }
  }
}
```

### Create

1. Create food by specifying 3 required args: name, placeOfOrigin, image
```bash
mutation {
  foodCreate(input: {
    name: "Apple Pie",
    placeOfOrigin: "US",
    image: "apple-pie.png"
  }) {
    id
    name
    placeOfOrigin
    image
  }
}
```
2. Create a nutrition fact for the new food with id#4
```bash
mutation {
  nutritionCreate(input: {
    foodId: 4,
    servingSize: "100 g",
    calories: "237 g",
    totalFat: "11 g",
    saturatedFat: "3.8 g",
    transFat: "0 g",
    cholesterol: "0 mg",
    sodium:"266 mg",
    potassium: "65 mg",
    totalCarbohydrate: "34g",
    dietaryFiber: "1.6 g",
    protein: "1.9 g",
    sugars: "36 g",
    vitaminA: "0%",
    vitaminC: "0%",
    iron: "6%",
    calcium:"1%",
  }) {
    id
    calories
    foodId
  }
}
```
verify
```bash
query {
  allFood{
    id
    name
    nutrition{
      foodId
      calcium
      protein
      totalFat
    }
  }
}
```

### Update

1. Update a name of a food item with id#4
```bash
mutation {
  foodUpdate(input: {
    id: 4,
    name: "Pumpkin Pie"
  }) {
    id
    name
  }
}
```
verify 
```bash
query {
  food(id: 4) {
    id
    name
  }
}
```
2. Update a nutrition fact with id#4
```bash
mutation {
  nutritionUpdate(input: {
    id: 4,
    calories: "300 g",
    protein: "2.2 g",
    totalFat: "12 g"
  }) {
    id
    calories
    protein
    totalFat
  }
}
```
verify
```bash
query {
  food(id: 4) {
    name
    nutrition {
      id
      foodId
      calories
      protein
      totalFat
    }
  }
}
```
### Delete

1.Delete a nutrition fact with id#4
```bash
mutation {
  nutritionDelete(input: {
    id: 4
  }) {
    id
  }
}
```
verify the nutrition of the food item is null
```bash
query {
  food(id: 4) {
    name
    nutrition {
      id
    }
  }
}
```
2. Delete a food item with id#4
```bash
mutation {
  foodDelete(input: {
    id: 4
  }) {
    id
  }
}
```
verify the query returns 3 items
```bash
query {
  allFood {
    id
  }
}
```
this one will throw an exception
```bash
query {
  food(id: 4) {
    id
  }
}
```
