# Application Data Modeling

## Application entities

![App Entities](./figures/app-entities.drawio.svg)

### `UserAccount`

This represents a single user account for the application. It is used mainly to associate clothing items and closet spaces that are owned by the user. To achieve this goal, the following data properties are needed:

- A unique user identifier 
- Information used for authentication (password, access token)
- A list of closet spaces that the user owns
- The profile details of the account specified in a composed object

An user account needs to be authenticated in order to perform operations in the application. Since this object only keeps direct track of the closet spaces owned by a user, the primary operations are `add_close_space` and `remove_closet_space`. When created, an account is given a default closet space. All clothing items associated with a user account are stored in a closet space that they own.

### `UserProfile`

This class is a place for user account details to be specified. These extra details include:

- A display name
- A profile image
- Statistics about the account (such as number of closet spaces owned to clothing items in use)

The operations provided by this class are mostly intended to modify or update the information about the user. Statistical information is not directly mutable from this object but should be refreshable from the profile object

### `ClosetSpace`

This class represents a storage place for clothing items and outfits to be placed into. A user may have 1 or more closet spaces associated with their account. A closet space may have 0 or more articles of clothing placed in it. Closet spaces have the following data properties:

- A name
- The unique user account identifier that owns the closet space
- A collection of clothing items that are placed in the closet space

A user can than add and remove articles of clothing from an existing closet space that they own.

### `ClothingArticle`

This is a abstract class that represents an article of clothing in a closet space. It provides and interface that all items in a closet space have in common. For data attributes, the following are included for every article of clothing within the application

- A name
- A set of labels (key-value pairs) associated with the item
- A time stamp of when the item was last worn. A newly created item has this set to `null`
- A value that describes the state of the clothing (i.e. it is available or it is soiled)

In terms of operations, the clothing article has an interface to do the following:

- Add or remove a label from the item
- Update the state of the clothing to a new given state
- An action to wear the item and set its last worn date to today

### `ClothingItem`

An extension of the `ClothingArticle` class that represents a singular piece of clothing that is in a closet space. In addition to the common fields, items also provide additional fields for the following

- The item's color
- The item's brand name
- The item's size
- The item's fabric
- The item's purchase date and price
- The item's image
- The item's usage slots (places the item can be worn)

This subclass implements the interface it inherits from and adds methods to update the new fields it adds on.

### `Outfit`

An extension of the `ClothingArticle` class that represents a set of clothing items that are worn together to create an predetermined style. In addition to the common fields, outfits also have the following information

- The outfit's layer composition (see `OutfitLayer` for more information)
- The outfit's total cost (typically the sum of the costs of the items used in the outfit)
- The outfit's images (a collection of the images of the items used in the outfit. An additional collected image can also be included)

This subclass implements the interface it inherits from and adds methods to modify the outfit such as:

- An action to use an item in a chosen slot
- Methods to add or remove layers from the outfit (of which there is always a single layer)

### `OutfitLayer`

This represents a single layer of clothing of an outfit. It provides slots that clothing can be placed into:

- Head
- Torso
- Legs
- Feet
- Hands

An article of clothing will specify must slots it can be placed in. Most items typically take a single slot, but if multiple slots are specified, the article takes up all of the slots (Ex. a full-body dress or gown might take up the torso and legs slot).

### `Enumerations`

Multiple enumerations are used to model certain data points. These include:

- Clothing sizes (extendable with user-defined sizes)
- Clothing fabrics (extendable with user-defined fabrics)
- Clothing slots
- Clothing readiness states

## Document models

### User document

```yaml
_id: UUID
displayname: String
avatar:
  media_server: IP
  resource: Path
hashed_password: String
closet_spaces: # 1 of more allowed with `default` always existing
  - name: default
    clothing: [ClothingID]
  - name: custom space 1
    clothing: [ClothingID]
user_sizes:
  - name: String
user_fabrics:
  <fabric_name>: # number fields must add up to 100%
    - material: String
      composition: number
    - material: String
      composition: number
```

### Clothing document

```yaml
_id: UUID
name: String
owner: UserID
labels: # 0 or more allowed
  <label-key>: <label-value>
last_worn: datetime | null
readiness: Available | Soiled | Stowed | Damaged | Discarded
type: Item | Outfit
```

For clothing items, the following addition fields are included

```yaml
color: String
brand: String | null
size: String | UserSize 
fabric: String | UserFabric
purchase:
  date: date
  cost: number
image:
  media_server: IP
  resource: Path
useable_slots:
  head: bool
  torso: bool
  legs: bool
  feet: bool
  hands: bool
```

For outfits, the following addition fields are included

```yaml
layers:
  1: # Must ensure that type is `Item` here
    head: ClothingID | null
    torso: ClothingID | null
    legs: ClothingID | null
    feet: ClothingID | null
    hands: ClothingID | null
  2: 
    ...
```
