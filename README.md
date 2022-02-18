# How to setup Geyser Custom Items?
First, before you do anything, you need to choose how you are going to register your item. Is it using custom model data (CMD) or using damage predicates? Do you want to use a json file or the Geyser API? Please choose your section accordingly, then move on to the resource pack creation part.

## JSON files
1. You need to run a version of Geyser that supports custom items. Then, normally, you should have a folder called `custom_mappings` that is created. That would be with the Geyser .jar file for standalone and inside you Geyser data folder for an extension.
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
4. Inside this java item, goes an array of all your custom items. This is where the custom model data and the damage predicates come in.
    * Custom model data:
    ```json
    {
        "name": "my_item",
        "custom_model_data": 1
    }
    ```
    * Damage predicates:
    ```json
    {
        "name": "my_item",
        "damage_predicate": 1
    }
    ```
5. You now have some modifiers that you can set to further customise your item. They are NOT required.
    * `display_name` (string)
    * `is_tool` (boolean)
    * `allow_offhand` (boolean)
    * `is_hat` (boolean)
    * `texture_size` (int)

## Geyser API
1. In your extension config file, you need to set: `loadTime: "PRE_INITIALIZE"` so that the pre init event can be caught, and your items can be registered.
2. Create your custom item, and store it somewhere:
```java
CustomItemData myItem = new CustomItemData(111111, "my_item");
```
3. You now have some modifiers that you can set to further customise your item. They are NOT required.
```java
myItem.setDisplayName("displayName");
myItem.setIsTool(false);
myItem.setAllowOffhand(false);
myItem.setIsHat(false);
myItem.setTextureSize(16);
```
4. Then, in your pre init event, you can register your item:
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
5. Then you need to put your textures inside `items`. Make sure to have it match the texture name that you pur in `item_texture.json`!