# Data Persistent System

## Script "DataPersistentHandler" for Persistent Data Saving

The `DataPersistentHandler` script is a `ScriptableObject` used to manage the saving and loading of persistent data in a game. It enables the saving and loading of objects that implement the `ISaveMonoBehavior` or `ISave` interfaces, both for scene instances and assets.

### Variables

- `scriptableObjectSaveables`: A list of all `ScriptableObjects` compatible with the saving system. These objects must be referenced in this list to be considered for saving and loading.

### `OnValidate()` and `GetAssets<T>()` Methods

The `OnValidate()` and `GetAssets<T>()` methods are used only in the Unity editor to update the `scriptableObjectSaveables` list with `ScriptableObject` objects that are compatible with the `ISave` interface. They retrieve `ScriptableObject` objects of the specified type and add them to the list.

### `SaveAll()` and `LoadAll()` Methods

The `SaveAll()` and `LoadAll()` methods are used to save and load all persistent data, respectively.

- `SaveAll()`: This method iterates through all `ISaveMonoBehavior` objects in the scene and `ScriptableObject` objects referenced in `scriptableObjectSaveables`. For each object, it calls the `OnSave()` method to obtain the data to be saved, converts it to JSON, and stores it in a file.
- `LoadAll()`: This method iterates through all `ISaveMonoBehavior` objects in the scene and `ScriptableObject` objects referenced in `scriptableObjectSaveables`. For each object, it loads the data from the corresponding file using the `OnLoad()` method.

### `ResetAll()` Method

The `ResetAll()` method is used to reset all `ScriptableObject` objects referenced in `scriptableObjectSaveables` by calling the `OnReset()` method of the `ISave` interface.

### `Save()` and `Load()` Methods

The `Save()` and `Load()` methods are static methods used to save and load data for an object that implements the `ISave` interface. They take the object to save or load and a file name as parameters.

- `Save()`: This method calls the `OnSave()` method of the object to obtain the data to be saved. Then, it converts the data to JSON and stores it in a file in the application's persistent data folder.
- `Load()`: This method loads the data from the corresponding file using the `OnLoad()` method of the object.

These methods utilize the `JsonUtility` class for JSON serialization and deserialization of data.

---

## DataPersistentUtility Utility Class

The `DataPersistentUtility` utility class contains several utility methods used by the `DataPersistentHandler` script to facilitate certain operations related to saving and loading persistent data.

### `ExtractAssetName()` Method

The `ExtractAssetName()` method is used to extract the name of a `ScriptableObject` object. It takes the `ScriptableObject` object from which to extract the name as a parameter and returns the name as a string.

### `GetAssetFromResources<T>()` Method

The `GetAssetFromResources<T>()` method is used to load an object from the "Resources" folder of the application. It takes the name of the object to load as a parameter and returns the object of the specified type. This method is used to load the `ScriptableObject` objects referenced in the `DataPersistentHandler` script.

### `GenerateID()` Method

The `GenerateID()` method is used to generate a unique identifier. It utilizes the `Guid` class to generate a unique identifier and returns its hash code.

### `TryGetSaveInterface()` Method

The `TryGetSaveInterface()` method is an extension for `ScriptableObject` objects. It checks if the object implements the `ISave` interface and returns the object cast to `ISave`. This method is used to retrieve the `ISave` interface from the `ScriptableObject` objects referenced in the `DataPersistentHandler` script.

---

The `DataPersistentUtility` utility class provides useful methods to extract the name of a `ScriptableObject` object, load objects from the "Resources" folder, generate unique identifiers, and retrieve the `ISave` interface from `ScriptableObject` objects. These methods are used by the `DataPersistentHandler` script to facilitate the management of persistent data in a game.

## ISave Interface

The `ISave` interface contains the following methods:

### `OnLoad(string data)` Method

The `OnLoad(string data)` method is used to implement the loading behavior for an object. It takes a string parameter containing the data in JSON format. You can use the `JsonUtility.FromJson()` method to parse the JSON data. Inside this method, you should implement the logic to load the data using the provided data.

### `OnSave(out SaveData saveData)` Method

The `OnSave(out SaveData saveData)` method is used to implement the saving behavior for an object. It takes an `out` parameter `saveData` of type `SaveData`. You need to populate this variable with the data to be saved. You can create a child class of `SaveData` if you want to add custom data.

### `OnReset()` Method

The `OnReset()` method is used to implement the reset behavior for an object. Inside this method, you should implement the logic to reset the object.

The commented example in the code shows how you can implement these methods for a `ScriptableObject`-derived object. The `OnLoad()` method parses the JSON data to load the data into the object, while the `OnSave()` method creates a `SlotsInventorySaveData` data structure containing the data to be saved. You can adapt these methods according to the needs of your own game.

---

The `ISave` interface is used to define the methods required for saving and loading data. Objects that implement this interface must provide logic for loading, saving, and resetting. This interface allows for better separation of concerns and facilitates the management of saving and loading operations for persistent data.
