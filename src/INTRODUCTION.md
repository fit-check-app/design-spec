# Fit Check Design Specification

Welcome to the fit check app design specification. This repository houses the design artifacts of an app codenamed "Fit check" that is being developed by a couple of friends. We have recently completed our computer science educations and are looking to obtain real-word experience before entering the workforce. We are aiming for simplicity and a focus on content, so we are utilizing [mdbook](https://github.com/rust-lang/mdBook) to manage presentation. 

## About the app

Fit check is a web app that aims to help users track the contents of their wardrobe and the things that they wear. The app will allow users to specify the items of clothing that they own and display those items in a user-friendly and searchable way. By defining clothing items, users can then specify which items they are wearing on a given day. This information can be used to track the frequency of items worn and the cleanliness of items (e.g. how long since the last wash). The app will also allow users to specify outfits, which are collections of clothing items that are worn together. The same tracking features will be made available for outfits as well.

### Features

This app aims to help users track their clothing. Some preliminary features include

- Clothing item specification
- Clothing item state
- Outfit specification
- Wear frequency statistics
- Cleanliness and damage tracking
- Suggestions for new items and outfits based on
  - Items owned and worn frequently
  - Items that are not present in the wardrobe (e.g. a user has shirts in every color except green, so the app can suggest green shirts)
  - Weather events (e.g. local weather is expected to be cold, so the app can suggest a coat)

The finer details of these features have yet to be refined. Any additional features have not yet been discussed

### Application type

We are aiming to develop a web app. This should allow for easy access across devices and platforms. Consideration for a native mobile app have not been made at this time. Instead, we are focusing on making the web application responsive and mobile-friendly so that the platform can be accessed on any device.
