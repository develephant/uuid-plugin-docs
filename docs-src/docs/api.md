![logo](img/logo128.png)

<h3>UUID Plugin API</h3>

## new

Create a new universally unique identifier.

```lua
UUID.new()
```

__Arguments__

_This method takes no arguments._

__Returns__

A new UUID as a __String__.

__Example__

```lua
local uuid = UUID.new()

print(uuid) --> 5D91AFDF-17D1-5942-9216-E593BD9EC80A
```

__Notes__

 - If you want to generate multiple UUIDs in one pass, ___do not___ use a `for` loop with this method. See the [`batch`](#batch) method instead.

---

## save

Create and save a persistent universally unique identifier to the device. The UUID is saved to the _Documents_ directory.

By default the filename is "_uuid", but can be changed by providing a name to the `filename` argument.

If a saved UUID already exists, you must pass the `overwrite` flag to save a new UUID.

```lua
UUID.save([overwrite,][ filename])
```

__Arguments__

|Name|Description|Type|Required|
|----|-----------|----|--------|
|overwrite|Generate a new UUID and overwrite the existing saved file.|_Boolean_|__N__|
|filename|An alternative name for the saved UUID. Defaults to "_uuid".|_String_|__N__|

__Returns__

On success, a new UUID as a __String__, or __nil__ and an error.

__Examples__

```lua
-- Create and save a UUID
local uuid = UUID.save()

-- Overwrite a saved UUID
local uuid = UUID.save(true)

-- Use a alternative name
local uuid = UUID.save("_userid")

-- Overwrite with the alternative name
local uuid = UUID.save(true, "_userid")

-- With error checking
local uuid, err = UUID.save([args])

if not uuid then
  print(err)
end
```

__Notes__

 - If you save a UUID with an alternative name and wish to overwrite it, you must pass the `overwrite` flag, as well as the alternative name it was saved with.

 - You can generate and save multiple UUIDs by using the `filename` argument with different values. Load them by passing the same name to the [`load`](#load) method.

---

## load

Load a [saved](#save) universally unique identifier from the device. The UUID is loaded from the _Documents_ directory.

By default the filename loaded is "_uuid", but can be changed by providing a name to the `filename` argument.

To guarantee a UUID will be loaded, you can pass the `create` flag to have a newly [saved](#save) UUID generated. If the file already exists, this flag is ignored.

```lua
UUID.load([create,][ filename])
```

__Arguments__

|Name|Description|Type|Required|
|----|-----------|----|--------|
|create|Generate a new [saved](#save) UUID if no file is found to load.|_Boolean_|__N__|
|filename|An alternative name to load the UUID from. Defaults to "_uuid".|_String_|__N__|

__Returns__

On success, the [saved](#save) UUID as a __String__, or __nil__ and an error.

__Example__

```lua
-- Load a saved UUID
local uuid = UUID.load()

-- Create a new UUID file if none exists
local uuid = UUID.load(true)

-- Load using an alternative name
local uuid = UUID.load("_userid")

-- Create a new UUID file if none exists
-- Use an alternative name for the file.
local uuid = UUID.load(true, "_userid")

-- With error checking
local uuid, err = UUID.load([args])

if not uuid then
  print(err)
end
```

__Notes__

 - If the UUID you want to load was [saved](#save) with an alternative name, you must pass that name to the `filename` argument when calling the `load` method.

---

## delete

Delete a [saved](#save) universally unique identifier from the device. The UUID is removed from the _Documents_ directory.

```lua
UUID.delete([filename])
```

__Arguments__

|Name|Description|Type|Required|
|----|-----------|----|--------|
|filename|An alternative filename used for deletion.|_String_|__N__|

__Returns__

On success, the __Boolean__ `true`, or __nil__ and an error.

__Example__

```lua
-- Delete a saved UUID
local uuid = UUID.delete()

-- Delete with the alternative name
local uuid = UUID.delete("_userid")

-- With error checking
local ok, err = UUID.delete([args])

if not ok then
  print(err)
end
```

__Notes__

 - If the UUID you want to delete was [saved](#save) with an alternative name, you must pass that name to the `filename` argument when calling the `delete` method.

---

## batch

Create multiple universally unique identifiers in one pass.

While UUIDs are generated with millisecond precision, if trying to produce multiple UUIDs using a `for` loop you _may_ generate a duplicate UUID. Use this method instead to guarantee a batch of unique ids.

```lua
UUID.batch(count)
```

__Arguments__

|Name|Description|Type|Required|
|----|-----------|----|--------|
|count|The amount of UUIDs to generate.|_Number_|__Y__|

__Returns__

A __Table__ array of UUIDs.

__Example__

```lua
--Generate 10 UUIDs
local uuids = UUID.batch(10)

for i=1, #uuids do
  print(uuids[i])
end
```

__Notes__

 - Batches are not saved to any file.
