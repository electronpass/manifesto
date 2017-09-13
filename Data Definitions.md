# Data Definition
This file explains the wallet format. It is basicly encrypted JSON.

## Current version
This document is describing version ```0```. This version

## Why JSON
There are probably better object serialization and storage options than JSON, but JSON has some major benefits:
- Future compatibility. If we add a new filed type old clients will work, because they just won't parse and display the new feature.
- Serialization is possible on almost every platform in almost every language.

This benefits make it easier to implement on other platforms than for instance kwallet and that's why the new standard. [Yes we know](https://xkcd.com/927/).

## Data on Disk
Password data is stored on disk as encrypted JSON. It's encrypted using ChaCha20-Poly1305 authenticated encryption. We are using libsodium library for encrypting the file and we recommend you do the same. JSON with passwords get encrypted and saved on disk as JSON:

```
{
  "timestamp": 1493189805,
  "version": 0,
  "data": "encrypted wallet"
}
```

- ```timestamp``` is a unix timestamp set to when the wallet was saved. It is used for merging 2 wallets (when syncing with).
- ```version``` represents version wallet, for backwards compaibility
- ```data``` is the actual wallet itself (encrypted)

## JSON Format
We are using JSON because it is flexible and allows us for future extensions. Unencrypted JSON never gets written to disk and only stays in RAM. Here is an example of a JSON file:

```
{
  "items": {
    "fed8f5d6744128839ed7390f84268a78": {
      "last_edited": 1493189705,
      "name": "Google",
      "fields": [
        {
          "name": "Username",
          "type": "username",
          "value": "open_user",
          "sensitive": false
        },
        {
          "name": "Password",
          "type": "password",
          "value": "secret_pa55",
          "sensitive": true
        }
      ]
    },
    "f26cfe5b0596bb5077db7f7c0e19d9e5": {
      "last_edited": 1493189650,
      "name": "Firefox",
      "fields": [
        {
          "name": "E-mail",
          "type": "email",
          "value": "electron.pass@mail.com",
          "sensitive": false
        },
        {
          "name": "Password",
          "type": "password",
          "value": "reallynotsecurepass123",
          "sensitive": true
        }
      ]
    }
  }
}

```

```Items``` is dictionary of passwords that user has stored. Item's key is an UUID. Each item has a ```name``` field which is a display name for the field and ```fields``` attribute which is an array of fields for the entry. Each field has 4 properties:

- ```name``` represents a display name, that is shown to the user
- ```type``` represents a field type. Types are explained in the next section of this document
- ```value``` represents the data that is stored in this field (for instance: the password itself)
- ```sensitive``` is a boolean that marks if the field's value should be hidden and only displayed with dots, unless the user explicitly requests to see the value

## Types
Type of the field is used to enhance the user experiance. It will probably not be used by the core library, but the applications will use it to show additional information to the user.

Currently possible types are:

- ```username``` for storing usernames
- ```password``` for storing passwords
- ```email``` for storing email addresses
- ```url``` for storing on which webpage the password entry is used
- ```pin``` for storing numeric values only
- ```date``` for storing dates
- ```other``` for storing other than what is listed above
