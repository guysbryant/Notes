---
tags:
- testing
- rspec
---
- [Terms, Keywords, Jargon, and Phrases](#terms-keywords-jargon-and-phrases)
- [Setup, Prerequisites](#setup-prerequisites)
  - [GEMS](#gems)
  - [Getting Started](#getting-started)
    - [spec_helper.rb](#spechelperrb)
    - [Rails_helper.rb](#railshelperrb)
  - [Configuring Database Cleaner](#configuring-database-cleaner)
  - [How to run the tests?](#how-to-run-the-tests)
- [Controller Tests](#controller-tests)
  - [Organizing Tests](#organizing-tests)
- [How to get your data ready?](#how-to-get-your-data-ready)
  - [Use Factories](#use-factories)
- [Test Basics](#test-basics)
  - [Matchers](#matchers)
    - [Equivalence](#equivalence)
    - [Boolean](#boolean)
    - [Regex](#regex)
    - [Comparison](#comparison)
    - [Classes and Types](#classes-and-types)
    - [Errors](#errors)
    - [Collection contents](#collection-contents)
    - [Predicate Matchers](#predicate-matchers)
  - [let && let!](#let--let)
  - [Subjects](#subjects)
  - [Callbacks](#callbacks)
  - [Generators](#generators)
  - [Tags](#tags)
  - [Test Speed](#test-speed)
  - [Spring Preloader](#spring-preloader)
    - [How to use](#how-to-use)

# Terms, Keywords, Jargon, and Phrases

- *RSpec* – BDD testing framework
- *bdd* – Behavior Driven Development
- *Capybara* – Web-based automation framework to create functional tests that simulate how users would interact with your application.
- spec_helper.rb / rails_helper.rb – Contain the default Rspec setup
- *Describe- – Wraps a set of tests against one functionality
- *Context block* – Wraps a set of tests against one functionality under the same state
- *Factory* – Prepares data for controller specs
- *Stubs* --
- *Spies* --
- *Fakes* --
- *Unit Test* – Model tests
- *Integration Test* – Work flow test, imitating user behavior, synchronously testing several components.
- *Spring Preloader* – Keeps Rails running in the background to speed up common development tasks

# Setup, Prerequisites

## GEMS

[database_cleaner](https://github.com/DatabaseCleaner/database_cleaner) </br>
[rspec-rails](https://github.com/rspec/rspec-rails) </br>
FactoryBot

## Getting Started

`rails generate rspec:install`

This will create the following files:

- .rspec
- spec
- spec/spec_helper.rb
- spec/rails_helper.rb

### spec_helper.rb

- Used to configure RSpec

> expectation.include_chain_clauses_in_custom_matcher_descriptions = true

- [Chain method](https://relishapp.com/rspec/rspec-expectations/docs/custom-matchers/define-matcher-with-fluent-interface) allows custom matcher descriptions and failure messages to include text for helper methods

> mocks.verify_partial_double = true

- [Partial doubles](http://relishapp.com/rspec/rspec-mocks/docs/verifying-doubles/partial-doubles) prevent you from stubbing any methods that don’t already exist on an object, it typo-proofs your mocks.

> config.shared_context_metadata_behavior = :apply_to_host_groups

- [Shared context metadata](http://www.rubydoc.info/github/rspec/rspec-core/RSpec%2FCore%2FConfiguration%3Ashared_context_metadata_behavior) configures how Rspec treats metadata passed as part of a shared example group definition.

> filter_run_when_matching

- [Filter run when matching](https://relishapp.com/rspec/rspec-core/docs/filtering/filter-run-when-matching) allows you to limit a spec run to individual examples or groups you tagged with :focus metadata

> disable_monky_patching!

- [Prevents Rspec from monkey patching](https://relishapp.com/rspec/rspec-core/v/3-5/docs/configuration/zero-monkey-patching-mode)

- Makes objects behave in tests as they would in “real” life.

> [default_formatter](https://relishapp.com/rspec/rspec-core/v/2-5/docs/command-line/format-option)

- Default formatter sets up the default format of your rspec tests that will be use if no formatter has been set.

> config.profile_examples

- [Profile examples](https://relishapp.com/rspec/rspec-core/docs/configuration/profile-examples) print the slowest examples and example groups at the end of the spec run
- Slows down your suite

> config.order = :random and kernel.srand config.seed

- Configure tests to run in a [random order](https://relishapp.com/rspec/rspec-core/v/3-5/docs/command-line/order)
- Keeps each test independent of one another

### Rails_helper.rb

- Used for sepc which depends on Rails
- Usually model and controller tests
- Also on every part of a Rails
- Requires spec_helper.rb to work

> fixture_path

- [File fixture](https://relishapp.com/rspec/rspec-rails/v/3-5/docs/file-fixture) is a normal file stored in spec/fixtures by default

> use_transactional_fixtures

- [Transactional](https://relishapp.com/rspec/rspec-rails/docs/transactions) fixtures allow records created for one test to be visible for the nest test

> infer_sepc_type_from_file_location!

- Allows to automatically [tag specs in directories](https://relishapp.com/rspec/rspec-rails/docs/directory-structure) with matching type metadata so that they have relevant helper available to them

> filter_rails_from_backtrace!

- [Backtrace filtering](https://relishapp.com/rspec/rspec-rails/docs/backtrace-filtering) is used to filter out lines in backtraces that come from Rails gems in order to reduce the noise in test failure output

## Configuring Database Cleaner

Used to make sure that test start with a clean slate.
Add the following to your rspec.configure block in rails_helper.rb

```[ruby]
  config.before(:suite) do
    DatabaseCleaner.clean_with(:truncation)
  end
```

## How to run the tests?

After configuration you are good to go.
Four ways to run tests:

1. Everything at once
    1. Bundle exec rspec
    2. rake
2. One Rspec package
    1. Bundle exec rspec spec/models
3. One Rspec file at a time
    1. Bundle exec rspec spec/models/story_spec.rb
4. One by one
    1. Bundle exec rspec spec/models/story_spec.rb:10

# Controller Tests

## Organizing Tests

- Create a context for each meaningful input and wrap it into a describe block
- Example:

```ruby
  describe "Stories" do
    describe "GET stories#index" do
      context "when the user is an admin" do
        it "should list titles of all stories"
      end

      context "when the user is not an admin" do
        it "should list titles of users own stories" do
      end

      context "when the user is logged in" do
        it "should render stories#index"
      end

      context "when the user is logged out" do
        it "should redirect to the login page"
      end
    end

    describe "POST stories#create" do
      context "with valid attributes" do
        it "should save the new story in the database"
        it "should redirect to the stories#index page"
      end

      context "with invalid attributes" do
        it "should not save the new story in the database"
        it "should render stories#new template"
      end
    end
  end
```

# How to get your data ready?

## Use Factories

FactoryGirl

Create – Create and persist object to the database with all its associations.

- Triggers model and database validations

Build – Will not persist the object but will still make requests to the database if the factory has associations.

- Triggers validations only for associated objects

Build_stubbed – Does not call the database at all.

- Creates and assigns attributes to an object to make it behave like an instantiated object.
- Provides a fake id and create_at.
- Associations, if any, will be created via build_stubbed too.
- Does not trigger any validations.

Examples:

```ruby
  FactoryBot.define do
    factory :profile do
      user
      sequence(:name) { |n| "Name#{n}" }
    end
  end

  describe Profile, type: :model do
    it 'creates valid profile' do
      expect(FactoryBot.create(:profile)).to be_valid
    end
  end
```

Four Phases of a Test

1. Setup
    - Prepare scenario
        - Just get the minimum amount of data needed
            - agent = Agent.create(name: 'James Bond')
            - mission = Mission.create(name: 'Moonraker', status: 'Briefed')

2. Exercise
    *	Runs the tests in the spec
    - status = mission.agent_status

3. Verification
    - Test the system against your own expectations
    - expect(status).not_to eq 'MIA')

4. Teardown
    - Resets the memory and database
    - This is automated

```ruby
  describe Agent, '#favorite_gadget' do

    it 'returns one item, the favorite gadget of the agent ' do
    # Setup
      agent = Agent.create(name: 'James Bond')
      q = Quartermaster.create(name: 'Q')
      q.technical_briefing(agent)

    # Exercise
      favorite_gadget = agent.favorite_gadget

    # Verify
      expect(favorite_gadget).to eq 'Walther PPK'

    # Teardown is for now mostly handled by RSpec itself
    end

  end
```

# Test Basics

## Matchers

### Equivalence

- .to eq
- .not_to eq

### Boolean

- .to be_truthy
  - Any expression which is true
- .to be true
  - Only true
- .to be_falsy
  - Any expression which is false
  - nil is counted as false
- .to be false
  - Only false

### Regex

- .to match()

###  Comparison

- .to be
- <
- >
- <=
- \>=

### Classes and Types

- .to be_an_instance_of
- .to be_a
- .to be_an

### Errors

- .to raise_error
  - Any error
- .to raise_error(ErrorClass, “Some error message”)
  - Specify both the error class and message
- .to raise_error(“Some error message”)
  - Specify just the message

### Collection contents

- .to start_with
  - Check first element in collection
- .to end_with
  - Check last element in collection
- .to include
  - Look for element in collection

### Predicate Matchers

- Rspec can dynamically create matchers for you.
  - Works with predicate methods in your models
    - They end with a question mark
    - Use the method name without the question mark in the test
    - Example

```ruby
#Method is called bond?
def bond?()
    self.name == “Bond”
end
```

```ruby
#Tests use the method name without the ? with each of the equivalence matchers, .to, and .not_to
it ‘is James Bond’ do
    agent = Agent.create(name: ‘James Bond’)
    expect(agent).to be_bond
end
it ‘isn’t James Bond’ do
    agent = Agent.create(name: ‘Q’)
    expect(agent).not_to be_bond
end
```

## let && let!

Helper methods, not variables

- let is lazily evaluated
  - Only evaluated when a spec uses it
- let! Is run regardless of being used by a spec or not
  - Time-consuming
  - Costly if over used
  - Sets up data for each test
- Both version are memoized
- Cached with the same scope
- Usage examples

```ruby
Describe Mission, ‘#prepare’, :let do
  let(:mission) {Mission.create(name: ‘Moonraker’)}
  let(:bond) {Agent.create(name: ‘James Bond’)}
  it ‘adds agents to a mission’ do
    mission.prepare(bond)
    expect(mission.agents).to include bond
  end
end
```

## Subjects

Use in place of a let

```ruby
describe Agent, '#status' do
  subject { Agent.create(name: 'Bond')  }

  it 'returns the agents status' do
    expect(subject.status).not_to be 'MIA'
  end
end
```

- Reduces code duplication but can lead to ambiguity.

## Callbacks

Hooks that should run before or after each test in the spec file or around each test.

- before(:each)
- Run before each test
- Can be used to setup a fresh variable for each test
- before(:all)
- Run once before all the tests run
- Less time to run
- after(:each) and after(:all)
- Just like their before counterparts but after

## Generators

- `rails g rspec:install`
- Setup rspec
- `rails g rspec:model some_model`
- Creates a spec for some_model
- `rails g rspec:controller some_controller`
- Creates a spec for some_controller
- Same for views but we don’t use Rspec for views`
- `rails g rspec:helper some_helper`
- Creates a spec for some_helper
- This is where you put tests for view helpers
- Need to run helper methods on a helper object to work.

```ruby
describe '#set_date' do
  helper.set_date
end
```

- Also :mailer, :feature, :integration
  - These are beyond the scope of the current material
- All of these add the require ‘rails_helper’ to the top of the test file
- And set the Rspec.describe to the appropriate type for the tests
  - Type :controller, :helper etc..

## Tags

- Create custom tags for tests
  - These can be applied to any tests
    - Even across multiple types and folders
  - Run all tests associated with a specific tag:
    - `rspec –tag <tagname>`
  - Run all tests except for those associated with a specific tag:
    - `rspec –tag ~<tagname>`
  - Run/Ignore multiple tags at once
    - `rspec –tag <tagname> --tag <tagname>`
    - `rspec –tag ~<tagname> --tag <tagname>`

## Test Speed

Its important to have a fast test suite so that you are more likely to keep running tests to get their feedback.
What slows down a test suite?

- Database calls
  - Faking the data and only using the faked data helps with this
  - Fake as much as possible!

## Spring Preloader

- Comes default with Rails since version 4.1

### How to use

- `spring status`
  - -in the terminal will tell you if its running or not
- `spring server`
  - to start
