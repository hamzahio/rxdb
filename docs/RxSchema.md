# RxSchema
Schemas define how your data looks. Which field should be used as primary, which fields should be used as indexes and what should be encrypted. The schema also validates that every inserted document of your collections matches to it. Every collection has its own schema.
With RxDB, schemas are defined with the [jsonschema](http://json-schema.org/)-standard so that you dont have to learn anything new.

## Example
In this example-schema we define a hero-collection with the following settings:
- the name-property is the **primary**. This means its an unique, indexed, required string which can be used to definitly find a single document.
- the color-field is required for every document
- the healthpoints-field must be a number between 0 and 100
- the secret-field stores an encrypted value
- the skills-attribute must be an array with objects which contain the name and the damage-attribute. There is an maximum of 5 skills per hero.
```json
{
    "title": "hero schema",
    "description": "describes a simple hero",
    "type": "object",
    "properties": {
        "name": {
            "type": "string",
            "primary": true
        },
        "color": {
            "type": "string"
        },
        "healthpoints": {
            "type": "number",
            "min": 0,
            "max": 100
        },
        "secret": {
            "type": "string",
            "encrypted": true
        },
        "skills": {
            "type": "array",
            "maxItems": 5,
            "uniqueItems": true,
            "item": {
                "type": "object",
                "properties": {
                    "name": {
                        "type": "string"
                    },
                    "damage": {
                        "type": "number"
                    }
                }
            }
        }
    },
    "required": ["color"]
}
```

## Create a collection with the schema
```js
myDatabase.collection('hero', myHeroSchema)
  .then(collection => console.dir(collection));
```

---------
If you are new to RxDB, you should continue [here](./RxCollection.md)
