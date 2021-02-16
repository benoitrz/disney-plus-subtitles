# Disney Plus Subtitles

Recipe for saving subtitle settings on Disney Plus web app.

## Why?

When watching different movies/series on the Disney Plus web interface, I noticed that the subtitle settings were reset frequently and I ended having to change it back manually too often...

## How it works?

First, we need to access the profile page.

Go to https://www.disneyplus.com/en-gb/edit-profiles and select the profile you wish to update.

A Bearer token is available within the window local storage.

```javascript
const tokenStorageKey = "__bam_sdk_access";
const getLocalBearerToken = () => {
  for (const key in window.localStorage) {
    if (key.startsWith(tokenStorageKey))
      bearer = JSON.parse(window.localStorage[key]).context.token;
  }
};
```

With this token, we can now access the profile endpoint.

```javascript
fetch("https://global.edge.bamgrid.com/accounts/me/active-profile", {
      method: "GET",
      headers: new Headers({
        Authorization: "Bearer " + bearer,
        "Content-Type": "application/json; charset=utf-8",
      }),
    })
      .then((response) => response.json())
      .then((data) => (...))
```

We can modify the profile object with our needed change and send it back to the endpoint.

```javascript
profile.attributes.languagePreferences.subtitlesEnabled = true;
```

```javascript
fetch(
  "https://global.edge.bamgrid.com/accounts/me/profiles/" + profile.profileId,
  {
    method: "patch",
    headers: new Headers({
      Authorization: "Bearer " + bearer,
      "Content-Type": "application/json; charset=utf-8",
    }),
    body: JSON.stringify(profile),
  }
);
```

Your subtitle settings should now persist over time!
