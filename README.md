# How do I setup Geyser custom items?
First, before you do anything, you need to choose how you are going to register your item. Do you want to use a json file or the Geyser API? Please choose your section accordingly, then move on to the resource pack creation part.

## JSON files
1. You need to run a version of Geyser that supports custom items. Then, normally, after running, you should have a folder called `custom_mappings` that is created. That would be with the folder of Geyser.jar for standalone and inside your Geyser data folder for an extension.
2. Create a `.json` file, it can be any name you like and as many files as you like. You don't need to make one file per item. Here is the structure of the file:
```json
{
    "format_version": "1.0.0",
    "items": {

    }
}
```
3. Inside the `items` entry, you can add your java item to extend:
```json
"minecraft:JAVA_ITEM": [

]
```
4. Inside this java item, goes an array of all your custom items.
```json
{
    "name": "my_item"
}
```
5. Then, you need to set your registrations, they can be stacked, so that all of the specified types need to match.
    * Custom model data: `custom_model_data` (int)
    * Damage predicate: `damage_predicate` (int) This is a fractional value of damage/max damage and not a number between 0 and 1.
    * Unbreaking: `unbreaking` (boolean)
6. You now have some extra modifiers that you can set to further customise your item. **Note that the following modifiers are NOT required.**
    * `display_name` (string)
    * `is_tool` (boolean)
    * `allow_offhand` (boolean)
    * `is_hat` (boolean)
    * `texture_size` (int)
    * `render_offsets` (object) It works as follows. Note that all the sub-objects are optional, except x, y and z. You can have for example only a main hand with a position, and noting else.
    ```json
    "render_offsets": {
        "main_hand": {
            "first_person": {
                "position": {
                    "x": 0,
                    "y": 0,
                    "z": 0
                },
                "rotation": {
                    "x": 0,
                    "y": 0,
                    "z": 0
                },
                "scale": {
                    "x": 0,
                    "y": 0,
                    "z": 0
                }
            },
            "third_person": {

            }
        },
        "off_hand": {

        }
    }
    ```

## Geyser API
1. In your extension config file, you need to set: `loadTime: "PRE_INITIALIZE"` so that the pre init event can be caught, and your items can be registered.
2. Then, create your custom registration type, to which you can add any number of the following registration types. They can be stacked, so that all of the specified types need to match.
```java
CustomItemRegistrationTypes registrationTypes = new CustomItemRegistrationTypes();
registrationTypes.customModelData(1);
registrationTypes.damagePredicate(1); //This is a fractional value of damage/max damage and not a number between 0 and 1.
registrationTypes.unbreaking(true);
```
3. Create your custom item, and store it somewhere:
```java
CustomItemData myItem = new CustomItemData(registrationTypes, "my_item");
```
4. You now have some modifiers that you can set to further customise your item. **Note that the following modifiers are NOT required.**
```java
myItem.setDisplayName("displayName");
myItem.setIsTool(false);
myItem.setAllowOffhand(false);
myItem.setIsHat(false);
myItem.setTextureSize(16);
myItem.setRenderOffsets(new CustomRenderOffsets(...));
```
5. Then, in your pre init event, you can register your item:
```java
this.geyserApi().customManager().getItemManager().registerCustomItem("minecraft:JAVA_ITEM", myItem);
```

# How do I make my bedrock resource pack?
1. Setup a basic bedrock resource pack. If you need help, you can find it [here](https://wiki.bedrock.dev/guide/project-setup.html#rp-manifest).
2. Make a `textures` folder.
3. Make an `item_texture.json` file like this:
```json
{
  "resource_pack_name": "MY_PACK_NAME_HERE",
  "texture_name": "atlas.items",
  "texture_data": {
    
  }
}
```
4. Inside texture data, you can add your items. The texture name must match the name that you set in your mappings.
```json
"my_item": {
    "textures": [
        "textures/items/my_item"
    ]
}
```
5. Then you need to put your textures inside the `textures/items`. Make sure to have it match the texture path that you specified in `item_texture.json`!