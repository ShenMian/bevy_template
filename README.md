# A Bevy game template

Template for a Game using the awesome [Bevy engine][bevy] featuring out of the box builds for Windows, Linux, macOS, Web (Wasm), Android, and iOS.

# What does this template give you?

* small example ["game"](https://niklasei.github.io/bevy_game_template/)
* easy setup for running the web build using [trunk] (`trunk serve`) 
* run the native version with `cargo run`
* workflow for GitHub actions creating releases for Windows, Linux, macOS, and Web (Wasm) ready for distribution
    * the same workflow creates development builds for the mobile platforms (two separate workflows can push to the stores after [some setup](#deploy-mobile-platforms))
    * push a tag in the form of `v[0-9]+.[0-9]+.[0-9]+*` (e.g. `v1.1.42`) to trigger the flow
    * WARNING: if you work in a private repository, please be aware that macOS and Windows runners cost more build minutes. **For public repositories the builds are free!**

# How to use this template?

 1. Click "Use this template" on the repository's page
 2. Look for `ToDo` to use your own game name everywhere
 3. [Update the icons as described below](#updating-the-icons)
 4. Start coding :tada:
    * Start the native app: `cargo run`
    * Start the web build: `trunk serve`
        * requires [trunk]: `cargo install --locked trunk`
        * requires `wasm32-unknown-unknown` target: `rustup target add wasm32-unknown-unknown`
        * this will serve your app on `8080` and automatically rebuild + reload it after code changes
    * Start the android app: `cargo apk run -p mobile`
        * requires following the instructions in the [bevy example readme for android setup][android-instructions]
    * Start the iOS app (see the [bevy example readme for ios setup instructions][ios-instructions])
        * Install Xcode through the app store
        * Launch Xcode and install the iOS simulator (check the box upon first start, or install it through `Preferences > Platforms` later)
        * Install the iOS and iOS simulator Rust targets with `rustup target add aarch64-apple-ios x86_64-apple-ios aarch64-apple-ios-sim`
        * run `make run` inside the `/mobile` directory

You should keep the `credits` directory up to date. The release workflow automatically includes the directory in every build.

### Deploy web build to GitHub pages

 1. Trigger the `deploy-github-page` workflow
 2. Activate [GitHub pages](https://pages.github.com/) for your repository
     1. Source from the `gh-pages` branch (created by the just executed action)
 3. After a few minutes your game is live at `http://username.github.io/repository`

To deploy newer versions, just run the `deploy-github-page` workflow again.

# Deploy mobile platforms

For general info on mobile support, you can take a look at [one of my blog posts about mobile development with Bevy][mobile_dev_with_bevy_2] which is relevant to the current setup.

## Android

Currently, `cargo-apk` is used to run the development app. But APKs can no longer be published in the store and `cargo-apk` cannot produce the required AAB. This is why there is setup for two android related tools. In [`mobile/Cargo.toml`](./mobile/Cargo.toml), the `package.metadata.android` section configures `cargo-apk` while [`mobile/manifest.yaml`](./mobile/manifest.yaml) configures a custom fork of `xbuild` which is used in the `release-android-google-play` workflow to create an AAB.

There is a [post about how to set up the android release workflow][workflow_bevy_android] on my blog.

## iOS

The setup is pretty much what Bevy does for the mobile example.

There is a [post about how to set up the iOS release workflow][workflow_bevy_ios] on my blog.

# Removing mobile platforms

If you don't want to target Android or iOS, you can just delete the `/mobile`, `/build/android`, and `/build/ios` directories.
Then delete the `[workspace]` section from `Cargo.toml`.

# Getting started with Bevy

You should check out the Bevy website for [links to resources][bevy-learn] and the [Bevy Cheat Book] for a bunch of helpful documentation and examples. I can also recommend the [official Bevy Discord server][bevy-discord] for keeping up to date with the development and getting help from other Bevy users.

# Known issues

Audio in web-builds can have issues in some browsers. This seems to be a general performance issue and not due to the audio itself (see [bevy_kira_audio/#9][firefox-sound-issue]).

# License

This project is licensed under [CC0 1.0 Universal](LICENSE) except some content of `assets` and the Bevy icons in the `build` directory (see [Credits](credits/CREDITS.md)). Go crazy and feel free to show me whatever you build with this ([@nikl_me][nikl-twitter] / [@nikl_me@mastodon.online][nikl-mastodon] ).

[bevy]: https://bevyengine.org/
[bevy-learn]: https://bevyengine.org/learn/
[bevy-discord]: https://discord.gg/bevy
[nikl-twitter]: https://twitter.com/nikl_me
[nikl-mastodon]: https://mastodon.online/@nikl_me
[firefox-sound-issue]: https://github.com/NiklasEi/bevy_kira_audio/issues/9
[Bevy Cheat Book]: https://bevy-cheatbook.github.io/introduction.html
[trunk]: https://trunkrs.dev/
[android-instructions]: https://github.com/bevyengine/bevy/blob/latest/examples/README.md#setup
[ios-instructions]: https://github.com/bevyengine/bevy/blob/latest/examples/README.md#setup-1
[mobile_dev_with_bevy_2]: https://www.nikl.me/blog/2023/notes_on_mobile_development_with_bevy_2/
[workflow_bevy_android]: https://www.nikl.me/blog/2023/github_workflow_to_publish_android_app/
[workflow_bevy_ios]: https://www.nikl.me/blog/2023/github_workflow_to_publish_ios_app/
