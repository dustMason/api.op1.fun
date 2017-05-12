# op1.fun API

https://op1.fun has an API to allow users to upload and download packs and
patches using 3rd party apps and tools. It's responses are modeled on the [JSON
API](http://jsonapi.org) spec.

## Authentication

All requests must include two headers, `X-User-Token` and `X-User-Email`. The
token is obtained by the user on their profile page on the site.

Example request:

```shell
curl -X GET \
  https://api.op1.fun/v1/users/dustmason/packs \
  -H 'x-user-email: email@example.com' \
  -H 'x-user-token: xxxxxxxxxxxxxx'
```

## Endpoints

### `https://api.op1.fun/v1/users/:username/patches`

Method: GET

Returns user data and list of related Pack and Patch ids. If `:username` matches
the authenticated user, private packs and patches will be included.

**Example Response**

```json
{
  "data": {
    "id": "dustmason",
    "type": "users",
    "attributes": {
      "username": "dustmason"
    },
    "relationships": {
      "packs": {
        "data": [
          {
            "id": "konapulse-pack",
            "type": "packs"
          }
        ]
      },
      "patches": {
        "data": []
      }
    },
    "links": {
      "self": "https://api.op1.fun/v1/users/dustmason",
      "patches": "https://api.op1.fun/v1/users/dustmason/patches",
      "packs": "https://api.op1.fun/v1/users/dustmason/packs"
    }
  }
}
```

-----

### `https://api.op1.fun/v1/users/:username/patches`

Method: GET

Returns all public patches for the given user. If `:username` matches the
authenticated user, private patches are also included.

**Example Response**

```json
{
  "data": [
    {
      "id": "rlnd_tr808",
      "type": "patches",
      "attributes": {
        "name": "RLND_TR808",
        "public": true,
        "patch-type": "drum",
        "license": "",
        "description": "Another all-time classic",
        "user-id": "dustmason"
      },
      "links": {
        "self": "https://api.op1.fun/v1/users/dustmason/patches/rlnd_tr808",
        "file": "https://op1fun.s3.amazonaws.com/uploads/patch/7/RLND_TR808.aif",
        "preview": "https://op1fun.s3.amazonaws.com/uploads/patch/7/RLND_TR808.aif.mp3"
      }
    }
  ]
}
```

-----

### `https://api.op1.fun/v1/users/:username/packs`

Method: GET

Returns all public packs for the given user. If `:username` matches the
authenticated user, private packs are also included.

**Example Response**

```json
{
  "data": [
    {
      "id": "micropops",
      "type": "packs",
      "attributes": {
        "name": "Micropops",
        "public": true,
        "description": "A bunch of my favorite old school drum machines",
        "user-id": "dustmason"
      },
      "relationships": {
        "patches": {
          "data": [
            {
              "id": "soundmaster_sr88",
              "type": "patches"
            },
            {
              "id": "cr8000",
              "type": "patches"
            },
          ]
        }
      },
      "links": {
        "self": "https://api.op1.fun/v1/users/dustmason/packs/micropops",
        "download": "https://api.op1.fun/v1/users/dustmason/packs/micropops/download"
      }
    }
  ]
}
```

-----

### `https://api.op1.fun/v1/users/:username/packs/:pack_id`

Method: GET

Returns data on the given pack and includes data for all associated patches. if `:username` matches the authenticated user, this may be a private pack.

**Example Response**

```json
{
  "data": {
    "id": "micropops",
    "type": "packs",
    "attributes": {
      "name": "Micropops",
      "public": true,
      "description": "A bunch of my favorite old school drum machines",
      "user-id": "dustmason"
    },
    "relationships": {
      "patches": {
        "data": [
          {
            "id": "soundmaster_sr88",
            "type": "patches"
          },
          {
            "id": "cr8000",
            "type": "patches"
          }
        ]
      }
    },
    "links": {
      "self": "https://api.op1.fun/v1/users/dustmason/packs/micropops",
      "download": "https://api.op1.fun/v1/users/dustmason/packs/micropops/download"
    }
  },
  "included": [
    {
      "id": "soundmaster_sr88",
      "type": "patches",
      "attributes": {
        "name": "SoundMaster_SR88",
        "public": true,
        "patch-type": "drum",
        "license": null,
        "description": null,
        "user-id": "kb6-de"
      },
      "links": {
        "self": "https://api.op1.fun/v1/users/kb6-de/patches/soundmaster_sr88",
        "file": "https://op1fun.s3.amazonaws.com/uploads/patch/2316/SoundMaster_SR88.aif",
        "preview": "https://op1fun.s3.amazonaws.com/uploads/patch/2316/SoundMaster_SR88.aif.mp3"
      }
    },
    {
      "id": "cr8000",
      "type": "patches",
      "attributes": {
        "name": "CR8000",
        "public": true,
        "patch-type": "drum",
        "license": null,
        "description": "One of my favorite classic drum machines.",
        "user-id": "dustmason"
      },
      "links": {
        "self": "https://api.op1.fun/v1/users/dustmason/patches/cr8000",
        "file": "https://op1fun.s3.amazonaws.com/uploads/patch/6/CR8000.aif",
        "preview": "https://op1fun.s3.amazonaws.com/uploads/patch/6/CR8000.aif.mp3"
      }
    }
  ]
}
```

-----

### `https://api.op1.fun/v1/users/:username/packs/:pack_id/download`

Method: GET

Generates a zip file for the given pack, then returns a publicly accessible URL
for the generated file. This will be a slow request for large packs, and will
time out after 60 seconds.

**Example Response**

```json
{
  "url": "https://op1fun.s3.amazonaws.com/pack/92/micropops.zip",
  "success": true
}
```

-----

### `https://api.op1.fun/v1/users/:username/patches/:patch_id`

Method: GET

Returns data on the given patch. if `:username` matches the authenticated user,
this may be a private patch.

**Example Response**

```json
{
  "data": {
    "id": "cr8000",
    "type": "patches",
    "attributes": {
      "name": "CR8000",
      "public": true,
      "patch-type": "drum",
      "license": null,
      "description": "One of my favorite classic drum machines.",
      "user-id": "dustmason"
    },
    "links": {
      "self": "https://api.op1.fun/v1/users/dustmason/patches/cr8000",
      "file": "https://op1fun.s3.amazonaws.com/uploads/patch/6/CR8000.aif",
      "preview": "https://op1fun.s3.amazonaws.com/uploads/patch/6/CR8000.aif.mp3"
    }
  }
}
```
