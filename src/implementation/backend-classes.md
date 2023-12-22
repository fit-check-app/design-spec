# Backend Class Diagrams

## Service workflows

![Service Workflow](./figures/service-workflow.drawio.svg)

## Classes

### Application entities

![App Entities](./figures/app-entities.drawio.svg)

#### `UserAccount`

This class represents an individual user of the wardrobe application. They can sign in with their credentials and performs various operations in the application. `UserAccount`s are identified by their UUID. Details of the account owner, such as a display name, are stored in a separate `UserProfile` object. Each user account has 1 or more closet spaces to place clothing items and outfits into. A `UserAccount` is created with a `default` closet space. Additional closet spaces can be added with individual names. There must be at least one closet space tied with a `UserAccount`.

#### `UserProfile`

This class is a place for user account details to be specified. It remains rudimentary in hopes of further expansion on the functionality of accounts in this application. 

#### `ClosetSpace`

This class represents a storage place for clothing items and outfits to be placed into. A user may have 1 or more closet spaces associated with their account. A closet space may have 0 or more articles of clothing placed in it.

#### `ClothingArticle`

This is a abstract class that represents an article of clothing in a closet space. It provides and interface that all items in a closet space have in common. For data attributes, this includes a name, any labels associated with the item, a date the article was last worn, and the readiness state of the item. For operations, a method to wear an article today, and methods to update the labels and state of the article are provided. The methods are abstract and should be implemented by concrete classes

#### `ClothingItem`

An extension of the `ClothingArticle` class that represents a singular piece of clothing that is in a closet space. In addition to the common fields, items also provide additional fields for the following

- The item's color
- The item's brand name
- The item's size
- The item's fabric
- The item's purchase date and price
- The item's image
- The item's usage slots (places the item can be worn)

#### `Outfit`

An extension of the `ClothingArticle` class that represents a set of clothing items that are worn together to create an predetermined style. In addition to the common fields, outfits also have the following information

- The outfit's layer composition (see `OutfitLayer` for more information)
- The outfit's creation timestamp
- The outfit's total cost (typically the sum of the costs of the items used in the outfit)
- The outfit's images (a collection of the images of the items used in the outfit. An additional collected image can also be included)

The outfit class provides a method to use an specific item in that outfit. It will attempt to place the item in the topmost layer that can accept it. If no existing layer can accept the item, a new layer is added. If the item is already being used, the action to add it to the outfit is blocked. Layers are generally managed automatically, so the only intervention that should be needed is removing the unwanted layers. Outfits must have at least 1 layer, so the last layer is not allowed to be deleted. An outfit may be empty.

#### `OutfitLayer`

This represents a single layer of clothing of an outfit. It provides slots that clothing can be placed into. These slots include the head, torso, legs, feet, and hands. An article of clothing will specify must slots it can be placed in. Most items typically take a single slot, but if multiple slots are specified, the article takes up all of the slots (Ex. a full-body dress or gown might take up the torso and legs slot). Layers can be validated to ensure that clothing items are being used appropriately

#### `Enumerations`

Multiple enumerations are used to model certain data points. These include:

- Clothing sizes (extendable with user-defined sizes)
- Clothing fabrics (extendable with user-defined fabrics)
- Clothing slots
- Clothing readiness states

### Application tooling
