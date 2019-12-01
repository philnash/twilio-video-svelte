# Twilio Video built in Svelte

This is a first attempt at creating a simple video chat application using [Twilio Video](https://www.twilio.com/docs/video) and [Svelte](https://svelte.dev) as the front end framework.

## Running the project

### What you'll need

- [Node.js](https://nodejs.org) installed
- A Twilio account (you can sign up for a [free Twilio account here](https://www.twilio.com/try-twilio))

### Getting started

Clone the project and change into the directory:

```bash
git clone https://github.com/philnash/twilio-video-svelte.git
cd twilio-video-svelte
```

Install the dependencies:

```bash
npm install
```

Copy the `.env.example` file to `.env`:

```bash
cp .env.example .env
```

Create an endpoint you can request an access token from. Check out this [Twilio Function template for Video Tokens as a starting point](https://github.com/twilio-labs/function-templates/tree/master/video-token).

Once you have that running, set the URL of the token endpoint as the `TOKEN_URL` in `.env.`.

Then start [Rollup](https://rollupjs.org):

```bash
npm run dev
```

Navigate to [localhost:5000](http://localhost:5000). You should see your app running.

### Building and running in production mode

To create an optimised version of the app:

```bash
npm run build
```

You can run the newly built app with `npm run start` which uses [sirv](https://github.com/lukeed/sirv).
