# 🔤 Supertramp

 > But please, tell me who I am
 >
 > _- Supertramp, The Logical Song_

Generate default avatars for your users, containing their initials, with a consistent background colour. Avatars are created as SVGs on the fly, with the background colour chosen based on the initials, meaning the background will stay the same through reloads.

The avatars are returned as a string containing the SVG markup, that can be embedded directly into your HTML. This means no images need to be created or stored, eliminating the need for an image manipulation library such as ImageMagick.

Extracted from [PresentDay](https://www.mypresentday.com), inspired by https://kukicola.io/posts/creating-google-like-letter-avatars-using-erb-generated-svgs/

## Installation

`gem install supertramp`

## Usage

```ruby
# Create an instance of Supertramp, and then read it as a string
avatar = Supertramp.new(name: 'Matt Bearman')

avatar.to_s
# => <?xml ... /svg>

"#{avatar}"
# => <?xml ... /svg>
```

### Arguments
**`name:`** _(string)_

_Required unless `initials:` is specified_

A name from which the initials will be extracted. Eg: `name: 'Super Tramp'` will create an avatar with the initials `ST`.

**`initials:`** _(string)_

_Required unless `name:` is specified_

The initials to be displayed in the avatar. Eg: `initials: 'ST'` will create an avatar with the initials `ST`.

**`background:`** _(string)_

_Optional_

Tha background colour for the avatar. If a background is specified it will take priority over the default background colour selection.

The `background` argument can be any valid SVG colour string (eg: 'red', '#ff0000', 'rgba(127, 0, 0, 0.5)'), and does not have to be one of the colours in the configured `colours` array.

**`shape:`** _(string)_

_Optional, default = "square"_

The shape of the avatar, can be one of **"square"**, **"circle"**, or **"rounded"** (a rounded rectangle). To avoid typos you should specify this using the constants defined in the `Supertramp::Avatar` class:
 - `Supertramp::Avatar::SQUARE`
 - `Supertramp::Avatar::CIRCLE`
 - `Supertramp::Avatar::ROUNDED`

You can also specify this globally using the `shape` config attribute. This defaults to **"square"**.

### Examples

```ruby
# Extracting initials from a name, default shape and colour choice
Supertramp.new(name: 'Super Tramp').to_s
```
![](examples/st.svg)

```ruby
# Specifying initials and shape, default colour choice
Supertramp.new(initials: 'mb', shape: 'circle').to_s
```
![](examples/mb.svg)

```ruby
# Extracting initials, specifying custom colour, specifying shape as a constant
Supertramp.new(name: 'custom colour', color: 'rgba(127, 0, 0, 0.5)', shape: Supertramp::Avatar::ROUNDED).to_s
```
![](examples/cc.svg)



## How it works

Outputs an SVG of a square with initials in the center. Initials can either be passed in as `initials`, or extracted from the `name` argument.

Background colour is chosen using a seeded random based on the initials, so the colour won't change on reload unless the initials also change.

## Configuration

Use `Supertramp.configure` to set global config:

```ruby
Supertramp.configure do |config|
  # Array of background colours that can be chosen
  # (Sorry, I'm English, but for my trans-Atlantic neighbours you can also use config.colors 😊)
  # Default: %w[#B91C1C #B45309 #047857 #1D4ED8 #6D28D9]
  config.colours = %w[red green blue]

  # Set to true to transform initials to uppercase, false will leave them as they are provided
  # Note: This also affects initials that have been extracted from the name parameter
  # Default: true
  config.uppercase = false

  # The shape of the avatars, see the `shape:` argument for more details
  config.shape = Supertramp::Avatar::CIRCLE
end
```

## Roadmap

Things I'd like to add next, somewhat prioritised:

 - [x] Alternative shapes (circle and rounded rectangle)
 - [ ] Custom random seed (eg user_id)
 - [ ] base64 output for use with `data:` URLs
 - [ ] Dark text when a light background is specified
 - [ ] Caching
 - [ ] Full font customisation
 - [ ] Custom SVG templates

## Contributing

 - Read [how to properly contribute to open source projects on GitHub](https://www.gun.io/blog/how-to-github-fork-branch-and-pull-request).
 - Fork the project.
 - Use a topic/feature branch to easily amend a pull request later, if necessary.
 - Write good commit messages.
 - Use the same coding conventions as the rest of the project.
 - Commit and push until you are happy with your contribution.
 - Add tests for your code and make sure they're passing, and that you have 100% test coverage (as reported when running `bundle exec rspec`)
 - If your change has a corresponding open GitHub issue, prefix the commit message with [Fix #github-issue-number].
 - Make sure the test suite is passing and the code you wrote doesn't produce RuboCop offences (`bundle exec rspec` and `bundle exec rubocop`).
 - Squash related commits together.
 - Open a pull request that relates to only one subject with a clear title and description in grammatically correct, complete sentences.
