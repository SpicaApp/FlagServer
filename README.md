# Spica Profile Flag Server

The server for the profile flags introduced in Spica 0.9.1 beta 16

## What even is this?

Wow, good question. It's a thing. And I actually don't know why I added it.
Essentially, it just shows a colored ring around your profile picture in Spica.
There are multiple rings with more coming soon (probably).
You can find all current flags [here](data/allowedRings.json).

## Installation

Spica Profile Flag Server uses MariaDB, so you need a working MariaDB instance before you can use this.

There are 2 ways you can install this:

### Docker

1. Clone the repo
2. Make sure docker and docker-compose is installed on your system
3. Rename `docker-compose-example.yml` to `docker-compose.yml`
4. **Optional but recommended:** Change the MariaDB root password (for the `db` and `spicaprofilering` instance!)
5. Run `docker-compose up`
6. Wait and get a coffee while it's loading ☕️

### Manually

1. Clone the repo
2. Setup a MariaDB instance on your server/computer
3. Rename `.env.example` to `.env` (the file might be invisible)
4. Fill out all fields inside the `.env` file
5. Run `npm i`
6. Run `npm start`

If you have Docker already installed, `docker-compose` is probably easier because you essentially just have to run 1 command (`docker-compose up`).

## How to use it

There are only 4 endpoints (well technically 5 because there's `/` but idc about that).

- GET `/credits`
  - Description: Get all credited users (allowed to get the "supporter" ring)
  - URL parameters: ---
  - Headers: ---
  - Body: ---
  - Responses:
    - 200:
      - "credits": Array of all uids
- GET `/rings`
  - Description: Get all rings currently allowed by the API
  - URL parameters: ---
  - Headers: ---
  - Body: ---
  - Responses:
    - 200:
      - "rings": Array of all rings
- GET `/:id`
  - Description: Get the profile ring for a user.
  - URL parameters:
    - `id`: UID of the user
  - Headers: ---
  - Body: ---
  - Responses:
    - 400 "badRequest": the id isn't a string
    - 404 "missingResource": there's no database entry for this user
    - 200:
      - "uid": UID of the user
      - "ring": Ring/Flag that the user selected
      - "createdAt": Don't worry about that, it's just when the database entry got created
- POST `/:id`
  - Description: Update the profile ring for a user.
  - URL parameters:
    - `id`: UID of the user
  - Headers:
    - "Authorization": Token that alles uses for authorization (used to make a request to `https://micro.alles.cx/api/me`)
  - Body (JSON):
    - "ring": any ring from the allowed [ring/flags list](data/allowedRings.json)
  - Responses:
    - 401 "badAuthorization": The token is invalid or isn't for the user with the UID `id`
    - 400 "badRequest": `id` or `ring` is missing | ring isn't part of the allowed ring list
    - 401 "notAllowed": you tried to select the "supporter" ring but you're not part of the ["spicaCredits" list](data/spicaCredits.json)
    - 200:
      - "uid": UID of the user
      - "ring": Ring/Flag that the user selected
      - "createdAt": Don't worry about that, it's just when the database entry got created

## Add to your own application

Idk why you would do that but ok. There's not much to pay attention to, but I'd be nice if you can add all flags that are supported.
Please make sure to use the right flag stripe colors, just google the flag and get the right color values for it.

If you wanna add the supporter flag, you obviously can't google "Spica supporter flag", so here is how it looks like:

4 Stripes, divided equally

Colors:

- Stripe 1: #F39C12
- Stripe 2: #8E44AD
- Stripe 3: #E74C3C
- Stripe 4: #3498DB

Make sure to show the "Support flag" option only if the user is allowed to use it.

## Contributing

I'm too lazy to add a description or file for how to contribute now, just be nice to everyone and don't try to add some stupid shit.
**Please do not try to add yourself to the credits list if you're not credited in Spica.**

## Questions

If you have any questions, feel free to contact me on [twitter](https://twitter.com/leabmgrt) or open an issue.

## License

The project is licensed under the MIT license. Please refer to the [LICENSE](LICENSE) file for more information
