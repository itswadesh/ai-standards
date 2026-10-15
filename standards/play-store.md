# Google Play listing and releases

## Who clicks what

- Anything that publishes is the owner's click: creating a release, sending for
  review, "Submit N changes for review". Prepare everything up to that point,
  save it as a draft, and tell the owner exactly which button to press.
  Automated agents are often blocked from those clicks anyway.
- Some changes publish at once without review, such as contact details in Store
  settings, after a "Publish change?" confirmation. Ask before making them, and
  only report one as done after the confirmation dialog has been accepted.
- Never type passwords, test-account credentials or payment details into Play
  Console.

## Listing copy (ASO)

- **App name (30 characters):** brand plus the main keyword and place, for
  example "Hiphop jewelry store".
- **Short description (80 characters):** the main service, the places and the
  differentiator, with keywords in natural order.
- **Full description (up to 4,000 characters):**
  - the main keywords in the first lines;
  - then plain sections: what you get, prices, areas, how it works, how to pay,
    contact.
  - No keyword stuffing, no claims you can't verify, and no promises the
    business can't keep (`content.md`).
- Keep the copy in the repo (`mobile/store/LISTING.md`) and paste it from there.
- Tags: pick the closest available categories; there may be none for your niche.
- The contact phone, email and website match the site's.
- If the owner rules out a keyword, leave it out everywhere.

## Graphics

- **Icon:** 512×512 PNG. It is uploaded separately and never comes from the app
  bundle. Re-upload it when the launcher icon changes.
- **Feature graphic:** 1024×500, with the brand on a plain or textured
  background and no small text.
- **Phone screenshots:** 1080×1920 (9:16). Use 4–8 that tell the flow in order:
  home, prices, choose, time, details, done. Generate them with a Playwright
  script against a local test database:
  - shoot the screen at 360×640 with a device scale factor of 3;
  - place it under a caption of up to two short lines in the brand's display
    font, on the brand background;
  - the captions sell benefits ("No sign-up. Phone and address.").
- **Tablet screenshots:** 7-inch at 1080×1920 and 10-inch at 1440×2560, shot at
  tablet widths. Upload them under distinct file names, because the 7-inch files
  are the same size as the phone ones.
- Use a test database for screenshot bookings, never production. Cancel or delete
  any test bookings afterwards.

## Play Console UI notes

- Assets: Add assets, then Upload, then Add, then remove the old ones. A slot
  that allows one asset needs the old one deleted.
- In the asset library, click the tile to select it, not its radio button. The
  order of your clicks is the order the assets are added. Removing an asset
  re-renders the list, so find the elements again after each removal.
- The listing editor's Save is in the bottom bar. Then go to Publishing overview
  and press "Submit N changes for review" (the owner's click).
- Data safety: save the draft explicitly. Answers saved question by question have
  been lost.
- Uploading a new build while one is in review asks to restart the review.

## Releases

- Production track, with countries and managed publishing as the owner chooses.
  With managed publishing off, an approved release goes live at once.
- Release notes are short and plain, in the listing's language.
- After a release goes live, check that the installed app behaves the same as the
  site.
