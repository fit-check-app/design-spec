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

## Data Schema

![Data Schema](./figures/app-entity-schema.drawio.svg)

### User table

```sql
CREATE TABLE UserAccounts (
  user_id UUID PRIMARY KEY,
  email VARCHAR(255) NOT NULL,
  passwd VARCHAR(255) NOT NULL
);
```

### File resource table

```sql
CREATE TABLE FileResources (
  file_id INT AUTO_INCREMENT PRIMARY KEY,
  file_hostname VARCHAR(255) NOT NULL,
  file_path VARCHAR(255) NOT NULL
);
```

### Profile table

```sql
CREATE TABLE UserProfiles (
  profile_owner UUID PRIMARY KEY,
  display_name VARCHAR(255) NOT NULL,
  profile_image INT NOT NULL,
  FOREIGN KEY (profile_owner) REFERENCES UserAccounts(user_id)
    ON DELETE CASCADE
  FOREIGN KEY (profile_image) REFERENCES FileResources(file_id)
    ON DELETE SET NULL
);
```

### Closet space table

```sql
CREATE TABLE ClosetSpaces (
  space_owner UUID NOT NULL,
  space_name VARCHAR(64) NOT NULL,
  PRIMARY KEY (space_owner, space_name),
  FOREIGN KEY (space_owner) REFERENCES UserAccounts(user_id)
    ON DELETE CASCADE
)
```

### Custom sizes table

```sql
CREATE TABLE CustomSizes (
  size_name VARCHAR(64),
  defined_by UUID NOT NULL,
  PRIMARY KEY(defined_by, size_name),
  FOREIGN KEY (defined_by) REFERENCES UserAccounts(user_id)
    ON DELETE CASCADE
);
```

### Custom fabrics table

```sql
CREATE TABLE CustomFabrics (
  fabric_name VARCHAR(64),
  defined_by UUID NOT NULL,
  PRIMARY KEY(defined_by, fabric_name)
  FOREIGN KEY (defined_by) REFERENCES UserAccounts(user_id)
    ON DELETE CASCADE
)
```

### Fabric compositions table

```sql
CREATE TABLE FabricCompositions (
  fabric_name VARCHAR(64),
  defined_by UUID NOT NULL,
  material_name VARCHAR(64) NOT NULL,
  material_percentage INT NOT NULL CHECK (material_percentage > 0 AND material_percentage <= 100)
  FOREIGN KEY (defined_by, fabric_name) REFERENCES CustomFabrics(defined_by, fabric_name)
    ON DELETE CASCADE
    ON UPDATE CASCADE
)
```

### Clothing states enumeration

```sql
CREATE TYPE ClothingStates AS ENUM ('AVAILABLE', 'SOILED', 'DAMAGED', 'STOWED', 'DISCARDED')
```

### Clothing articles tables

```sql
CREATE TABLE ClothingArticles (
  owner UUID NOT NULL,
  space_placement VARCHAR(64) NOT NULL,
  name VARCHAR(64) NOT NULL,
  state Clothingstates DEFAULT 'AVAILABLE',
  last_worn DATE,
  color VARCHAR(32),
  brandname VARCHAR(64),
  size VARCHAR(64),
  fabric VARCHAR(64),
  purchase_date DATE,
  purchase_price DECIMAL(10, 2),
  image INT,
  covers_head BOOLEAN DEFAULT FALSE,
  covers_torse BOOLEAN DEFAULT FALSE,
  covers_legs BOOLEAN DEFAULT FALSE,
  covers_feet BOOLEAN DEFAULT FALSE,
  covers_hands BOOLEAN DEFAULT FALSE,
  PRIMARY KEY (owner, space_placement, name),
  FOREIGN KEY (owner, space_placement) REFERENCES ClosetSpaces(space_owner, space_name)
    ON DELETE CASCADE
    ON UPDATE CASCADE,
  FOREIGN KEY (owner, size) REFERENCES CustomSizes(defined_by, size_name)
    ON DELETE SET NULL
    ON UPDATE CASCADE,
  FOREIGN KEY (owner, fabric) REFERENCES CustomFabrics(defined_by, fabric_name)
    ON DELETE SET NULL
    ON UPDATE CASCADE,
  FOREIGN KEY (image) REFERENCES FileResources(file_id)
    ON DELETE SET NULL
    ON UPDATE CASCADE
);
```
### Clothing outfits table

```sql
CREATE TABLE Outfits (
  owner UUID NOT NULL,
  space_placement VARCHAR(64) NOT NULL,
  name VARCHAR(64) NOT NULL,
  state Clothingstates DEFAULT 'AVAILABLE',
  last_worn DATE,
  PRIMARY KEY (owner, space_placement, name),
  FOREIGN KEY (owner, space_placement) REFERENCES ClosetSpaces(space_owner, space_name)
    ON DELETE CASCADE
    ON UPDATE CASCADE
);
```

### Outfit layers table

```sql
CREATE TABLE OutfitLayers (
  owner UUID NOT NULL,
  space_placement VARCHAR(64) NOT NULL,
  outfit_name VARCHAR(64) NOT NULL,
  layer_index INT NOT NULL,
  head_slot_name VARCHAR(64),
  torse_slot_name VARCHAR(64),
  legs_slot_name VARCHAR(64),
  feet_slot_name VARCHAR(64),
  hands_slot_name VARCHAR(64),
  PRIMARY KEY (owner, space_placement, outfit_name, layer_index),
  FOREIGN KEY (owner, space_placement) REFERENCES ClosetSpaces(space_owner, space_name),
    ON DELETE CASCADE
    ON UPDATE CASCADE
  FOREIGN KEY (owner, space_placement, head_slot_name) REFERENCES ClothingArticles(owner, space_placement, name)
    ON DELETE SET NULL
    ON UPDATE CASCADE
  FOREIGN KEY (owner, space_placement, torse_slot_name) REFERENCES ClothingArticles(owner, space_placement, name)
    ON DELETE SET NULL
    ON UPDATE CASCADE
  FOREIGN KEY (owner, space_placement, legs_slot_name) REFERENCES ClothingArticles(owner, space_placement, name)
    ON DELETE SET NULL
    ON UPDATE CASCADE
  FOREIGN KEY (owner, space_placement, feet_slot_name) REFERENCES ClothingArticles(owner, space_placement, name)
    ON DELETE SET NULL
    ON UPDATE CASCADE
  FOREIGN KEY (owner, space_placement, hands_slot_name) REFERENCES ClothingArticles(owner, space_placement, name)
    ON DELETE SET NULL
    ON UPDATE CASCADE
);
```
