# Electronpass Manifesto
This file will try to explain some basic ideas behind the electronpass project. It will also explain the project structure and wallet format.

## The Why
Most important question is of course why. Passwords are essential part of our digital lives. Managing them however, is difficult. You can't remember every password if they are even slightly secure. The obvious thing to do is use a password manager. Not so obvious is which password manager to use. The ones that work everywhere (computer, phone, tablet..) are closed source payable products. There is nothing wrong with that, but we believe that there should always be an open source alternative. But in the field of password managers there is no good open source alternative that would work on every platform and have user friendly interface. And that's the basic idea behind electronpass: **open source, user friendly password manager.**

## The How
So how will we achieve that? The basic idea is that there is a unified wallet format which can be used on all systems and all languages. The format is explained in detail in [Data Definitions.md](Data Definitions.md).

## The project itself
We will try to maintain the wallet format and evolve the definition as the time goes on. We will also try to maintain 3 applications:
- Desktop application written in c++ ([electronpass-desktop](https://github.com/electronpass/electronpass-desktop))
- Android application written in Kotlin ([electronpass-android](https://github.com/electronpass/electronpass-android))
- iOS application written in Swift

The idea is that every application has its own "library" repo as a subrepository that will actually serialize the wallet file. Library is in quotes because it will not actually be linked as a library but is instead just the sources that get included in the client itself. The reason(s) for that is that each client needs its own backend. So we could just include the backend for serialization into the application repository itself, but we put it in a separate one for the purpose of making it easier for other people to use it in their own projects or implement it in different languages. We are not making it a dynamic or static library because of the problems with linking. We tried that and it did NOT work out. It was a complete disaster. Windows compile was non existent so the app only worked on linux and mac which means it's not user friendly.

## Contributing
For now we don't think that anyone will try to contribute to the project since it's so small. But if you do (thank you), the general idea is: look at the manifesto, understand the wallet format and then contribute to one of the existing clients by fixing the issues, or write your own client. If you are writing your own look at the libelectronpass repositories for some general guidelines for wallet format implementation.
