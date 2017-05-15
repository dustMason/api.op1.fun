---
title: api.op1.fun

toc_footers:
  - <a href='https://op1.fun'>op1.fun</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

---

# Introduction

https://op1.fun has an API to allow users to upload and download packs and
patches using 3rd party apps and tools. It's responses are modeled on the [JSON
API](http://jsonapi.org) spec.

Please [contact me](mailto:jordan@op1.fun) if you plan to build an API client of
any kind!

# Authentication

All requests must include two headers, `X-User-Token` and `X-User-Email`. The
token is obtained by the user on their profile page on the site.

# User

## Get User

`GET https://api.op1.fun/v1/users/:username`

Returns user data and list of related Pack and Patch ids. If `:username` matches
the authenticated user, private packs and patches will be included.

> Example Response

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

# Patches

## Get Patches

`GET https://api.op1.fun/v1/users/:username/patches`

Returns all public patches for the given user. If `:username` matches the
authenticated user, private patches are also included.

> Example Response

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

## Get Patch

`GET https://api.op1.fun/v1/users/:username/patches/:patch_id`

Returns data on the given patch. if `:username` matches the authenticated user,
this may be a private patch.

> Example Response

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

## Create Patch

`POST https://api.op1.fun/v1/patches`

Creates a new patch belonging to the authenticated user. Content-Type must be
`application/json` and file content must be Base64 encoded with this prefix:

`data:audio/x-aiff;base64,`

> Example Request

```shell
curl -X POST \
  https://api.op1.fun/v1/patches \
  -H 'accept: application/json' \
  -H 'content-type: application/json' \
  -H 'x-user-email: user@example.com' \
  -H 'x-user-token: xxxxxxxxxxxxxxxxxxxx' \
  -d '{
    "data": {
      "type": "patches",
      "attributes": {
        "name": "My Patch",
        "public": true,
        "description": "This is my patch",
        "license": "CC BY",
        "file": "data:audio/x-aiff;base64,<BASE64_ENCODED_FILE>."
        
      }
    }
  }'
```

Replace `<BASE64_ENCODED_FILE>` with a Base64 encoded patch AIFF file. Note that
any newline characters must be represented as `\n` as per the JSON spec.

License may be blank or one of: "CC BY", "CC BY-SA", "CC BY-NC", "CC BY-ND"

> Example Response

```json
{
  "data": [
    {
      "id": "new-patch",
      "type": "patches",
      "attributes": {
        "name": "New Patch",
        "public": true,
        "patch-type": "drum",
        "license": "CC BY",
        "description": "This is my patch",
        "user-id": "dustmason"
      },
      "links": {
        "self": "https://api.op1.fun/v1/users/dustmason/patches/new-patch",
        "file": "https://op1fun.s3.amazonaws.com/uploads/patch/7/new-patch.aif",
        "preview": "https://op1fun.s3.amazonaws.com/uploads/patch/7/new-patch.aif.mp3"
      }
    }
  ]
}
```

# Packs

## Get Packs

`GET https://api.op1.fun/v1/users/:username/packs`

Returns all public packs for the given user. If `:username` matches the
authenticated user, private packs are also included.

> Example Response

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

## Get Pack

`GET https://api.op1.fun/v1/users/:username/packs/:pack_id`

Returns data on the given pack and includes data for all associated patches. if `:username` matches the authenticated user, this may be a private pack.

> Example Response

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

## Download Pack

`GET https://api.op1.fun/v1/users/:username/packs/:pack_id/download`

Generates a zip file for the given pack, then returns a publicly accessible URL
for the generated file. This will be a slow request for large packs, and will
time out after 60 seconds.

> Example Response

```json
{
  "url": "https://op1fun.s3.amazonaws.com/pack/92/micropops.zip",
  "success": true
}
```
