Foundation Markup
-----------------

This chapter could be called a cut-and-paste chapter in that I will provide tried-and-true code that you can cut and paste into your application to build your projects foundation markup. In the next chapter we will add styles, but for now these are the steps necessary to start your Rails application out right in terms of foundation markup and from the front end view coders perspective. Let's get started.

### Groundwork

Assuming you just created a brand spanking new Rails application, these are the steps I follow...

    $ rails new <name> --no-test-framework
    Switch to new project folder.
    $ git init
    $ git add .
    $ git commit -am "Initial commit."
    $ git remote add origin git@github.com:<Github account>/.git
    Create a new repo at Github.
    $ git push -u origin master

...you should set up your Gemfile and .gitignore files right off the bat as follows:

#### Gemfile

Your Gemfile will change radically throughout the lifespan of your project. With every new application Rails generates one with a lot of commented out lines. You don't need these, they take up a lot of space and clutter things up. Without comments, in the new Rails app here is what you're left with:

    source 'https://rubygems.org'

    gem 'rails', '3.2.8'
    
    gem 'sqlite3'
    
    group :assets do
      gem 'sass-rails',   '~> 3.2.3'
      gem 'coffee-rails', '~> 3.2.1'
      gem 'uglifier', '>= 1.0.3'
    end
    
    gem 'jquery-rails'

To this I'm going to add gems that I know for certain I will work with. Borrowing from the best – I use Michael Hartl's "[Ruby on Rails Tutorial][RoR Tutorial]" Gemfile example – I start my projects with the following [Gemfile][]:

    source 'https://rubygems.org'
    
    gem 'rails', '3.2.8'
    
    group :development, :test do
      gem 'sqlite3',      '1.3.5'
      gem 'rspec-rails',  '2.11.0'
    end
    
    # Gems used only for assets and not required
    # in production environments by default.
    group :assets do
      gem 'sass-rails',   '3.2.5'
      gem 'coffee-rails', '3.2.2'
      gem 'uglifier',     '1.2.3'
    
      # Compass specific gems.
      gem 'compass-rails'
      gem 'oily_png'
    end
    
    gem 'jquery-rails',   '2.0.2'
    gem 'haml-rails'

    # See https://github.com/ndbroadbent/turbo-sprockets-rails3
    # gem 'turbo-sprockets-rails3'

    group :test do
      gem 'capybara',     '1.1.2'
    end
    
    group :production do
      gem 'pg',           '0.12.2'
    end

Since I typically use compass, I added the compass-rails and oily_png gems here. You may not want to, but I think it's smart choice if you do.

NOTE: Michael Hartl recommends using the following flag on your first bundle:

    $ bundle install --without production

Doing so installs your Gemfile gems, but prevents the installation of the production gems. You only have to do this once.

Finally, you should set up rspec now:

    $ rails generate rspec:install

For faster asset precompiles check out:

- [Turbo Sprockets for Rails 3.2.x][Turbo Sprockets]

#### .gitignore

For my [.gitignore][] file here is what I use; mostly borrowed from [HTML 5 Boilerplate][H5BP .gitignore]:

    # Ignore bundler config
    /.bundle

    # Ignore the default SQLite database
    /db/*.sqlite3

    # Ignore all logfiles and tempfiles
    /log/*.log
    /tmp

    # Numerous always-ignore extensions
    *.diff
    *.err
    *.orig
    *.log
    *.rej
    *.swo
    *.swp
    *.vi
    *~
    *.sass-cache

    # OS or Editor folders
    .DS_Store
    Thumbs.db
    .cache
    .project
    .settings
    .tmproj
    *.esproj
    nbproject
    *.sublime-project
    *.sublime-workspace

    # Dreamweaver added files
    _notes
    dwsync.xml

    # Komodo
    *.komodoproject
    .komodotools

    # Folders to ignore
    .hg
    .svn
    .CVS
    intermediate
    publish
    .idea
    doc/

    # Local
    scratch.*
    public/source

The last section "Local" contains files or folders I use to save things within the application, but only on my local machine:

- __scratch.*__ - I use this as a code graveyard; snippets of code I am no longer using but not yet ready to completely get rid of.

- __public/source__ - A folder for original third-party files or source code integrated into my application; Photoshop files; original images; basically the original copies of where things came from.

Some additional useful .gitignore ideas:

- [Ignore files][]
- [A Collection of Useful .gitignore Templates][.gitignore]

#### Deployment

If you use Heroku like I do run the following commands:

    $ heroku create --stack cedar
    $ git push heroku master
    $ heroku rename <new name>

### The Application Layout

As a front end person, sometimes called an Information Architect, when I think about layout I literally think about how a site is laid out on a screen. I don't think in terms of code, but more so in terms of organization of information and function for an end-user's consumption. In this role when speaking about "layout" I might say something like, "Wow! That's a great layout," or "I don't like the layout of this site, it's too confusing, maybe you should move this over there."

When I switch hats, as a front end coder I look at layout as the Rails view framework in which all of my front-end code lives and interacts with the outside world via different browsers and devices. In other words it's what a desktop or mobile browser takes and turns into something visual that an end user can understand and interact with, i.e. consume.

As a Rails front end developer, you will wear both hats, plus take on a third role in which you facilitate the handshake between the backend and the front end of your application. To be successful you must understand how the code you produce will be used by your colleagues (backend engineers). You will need to be able to reach into your models and controllers via instance variables, helper methods, Ruby via erb, Rails helpers, and you will need to think in terms of usability and work in design tools such as Photoshop, not to mention be a master in your own realm: HTML5, CSS3, JavaScript/jQuery, but before you do these things you need to layout a solid foundation to work within.

#### A Framework within a Framework

As much as Rails is a framework, within this framework lives a tinier front end framework; your foundation markup. Until Rails 3.0, where this foundation "lived" and the conventions for using it were very much a no man's land. It was a disorganized dumping ground for HTML, CSS, JavaScript, and Ruby: the wild wild West of coding. Since Rails 3.0 things have become "civilized" and within these new conventions is where we will begin to build our layout, the application layout.

NOTE: I first heard the view layer referred to as a "no man's land" and the "West" in John Athayde and Bruce Williams' preface to "[The Rails View: Create a Beautiful and Maintainable User Experience][The Rails View]". These guys are masters in this subject and I highly recommend reading their book.

So what is this tiny view framework within a framework? Well, it is predominantly HTML organized in specific folders of your application with styles and interactivity added via CSS and JavaScript (and Flash, but less and less these days). In Rails the heart of this view framework and all the code related to it lives in what we refer to as the layout template, or application.html.haml, which is typically broken up into different related files called partials which are all being pulled together into the whole. View code from other parts of the application for the most part pass through this layout and become framed by the layout template (with styles and JavaScript pulled in) before being rendered to the end-user.

#### Where Does It Live?

The front end structure I'm describing primarily lives in two high-level folders within your Rails application: the helpers and the views folders. The views folder is where the real action takes place and can be further subdivided into the Layout and Shared folders, which are home to the majority of your foundation front end code. Here's what it looks like and where you'll find these folders in any Rails 3.0 or greater project:

Project
  - assets
  - controllers
  - **helpers**
  - mailers
  - models
  - app
    - **views**
      - **layout**
      - **shared**

### The Code

Our goal is to write and organize the components of our layout in such a way that different browsers and devices can consistently, correctly, and efficiently display visual information to the end-user, and backend coders can understand and plug into it with ease. To help you along this path you can grab my [starter code][]:

- https://github.com/maxxiimo/base-haml

This is an implementation of [HTML5 Boilerplate][] code in haml and arranged for a Rails project. 

[TODO: update implementation to new version of boilerplate]

NOTE: I have included some necessary asset folders and files that coincide with the defaults I will provide in this chapter. Since I'm using modernizr, I add the require in application.js as follows:

    //= require jquery
    //= require jquery_ujs
    //= require modernizr-2.5.3.min
    //= require_tree .

##### About HTML5 Boilerplate

In coding copy and learn from the best, improve, then give back. I find that the best place to reference when building front end view templates is [HTML5 Boilerplate][]. This resource is an ongoing collaboration between expert front-end developers and the community.

To better understand it (and my implementation of it), the following sites are also worth a visit:

- [An Unofficial Guide to the HTML5 Boilerplate][Unofficial Guide]
- [Guide to HTML5 Boilerplate for Rails Developers][H5BP for Rails]

#### Where Do Things Go?

Code wise you have access to everything I use when starting a new application, my [sarter code][]. Structurally, these files fall into place as follows:

Project
  - assets
    - images
      - **fixtures**
      - **icons**
      - **logos**
      - **pics**
    - **javascripts**
  - controllers
  - helpers
    - [**application_helper.rb**][application_helper]
  - mailers
  - models
  - app
    - views
      - layout
        - [**_chromeframe.html.haml**][_chromeframe]
        - [**_head.html.haml**][_head]
        - [**_scripts.html.haml**][_scripts]
        - [**application.html.haml**][application]
      - shared
        - [**_footer.html.haml**][_footer]
        - [**_logo.html.haml**][_logo]
        - [**_navigation.html.haml**][_navigation]

Once you have downloaded the [starter code][] and placed all the files where they belong, you'll need to prep your app.

#### Prep the App

Prep your app by following these steps:

**Step 1** - Delete the default index.html file in your Public folder, and the rails.png image in your Assets/Images folder, as well as the app/views/layouts/application.html.erb file you are replacing with application.html.haml.

**Step 2** - If you do not have a page for your routes to go to, which I'm assuming you don't if this is a brand-new application, create one now. I'm going to generate a Pages controller now to match my footer links:

    $ rails generate controller Pages home about site_map terms privacy contact --no-test-framework

Make sure to delete the pages.css.scss file generated by Rails, we won't be using it.

NOTE: If you're not sure what I just did there you should really consider getting Michael Hartl's "[Ruby on Rails Tutorial][RoR Tutorial]".

**Step 3** - Finally, change your default route to whatever you want your application to default to. In the [starter code][] it's:

    # You can have the root of your site routed with "root"
    # just remember to delete public/index.html.
    root :to => 'pages#home' 

If you run WEBrick, your application should now just work.

    $ rails server

### Our Foundation

Although not apparent yet, i.e. visually, we have created a top-notch markup foundation for your application. With everything in place, and given that you followed Step 2 above, your homepage should look something like this: 

![][Basic HTML]

Not very attractive! ...but don't worry we'll address that in the [next chapter][Foundation Styles].

### Organization

Let's dissect what we've done.

#### Partials

All of the files in our [starter code][] layouts and shared folders could all be located in one file: [layout/application.html.haml][application], but rather than do this, to keep things organized and readable we separated the file into partials:

view/layout

1. [_head.html.haml][_head]
2. [_scripts.html.haml][_scripts]

view/shared

3. [_logo.html.haml][_logo]
4. [_navigation.html.haml][_navigation]
5. [_footer.html.haml][_footer]

We locate partials 1 and 2 in the same layout folder as application.html.haml -- NOT in the shared folder --- because they are essentially integral to the layout/application.html.haml file. Some people like to locate them in the shared folder. To pull them into application.html.haml  I like to use helper methods:

    def head
      render :partial => 'layouts/head'
    end
    
    def scripts
      render :partial => 'layouts/scripts'
    end

I think it looks cleaner in application.html.haml versus using render partial there.

Locate partials 3 - 5 in the shared folder. I separate logo and navigation into their own partials rather than use something like _banner.html.haml or _header.html.haml to hold both because often times logos and navigation vary between clients or functional areas within an application. When you start  the project you don't necessarily see this coming, but needs do change so you might as well lay them out in such a way that you can easily get at them in the future.

If you use my [starter code][], or follow my advice, your application.html.haml file will look like:

    !!!
    = head
    %body
      %header{:id => 'hd', :role => "banner"}
        = render :partial => 'shared/logo'
        = render :partial => 'shared/navigation'
      #main{:role => "main"}
        = yield
      = render :partial => 'shared/footer'
    = scripts

Concise and simple. Also note the use of [ARIA roles][]. It's good practice to always consider users that require assistive technology.

#### JavaScript

I don't think I have found a more comprehensive overview on this than Daniel Kehoe's article: "[Unholy Rails: External Scripts, jQuery Plugins, and Page-Specific JavaScript][Unholy Rails]"

##### The Scripts Partial

Generally it is best to put JavaScript at the very bottom of application.html.haml. Doing so will allow the page to render before scripts are loaded, but some scripts such as modernizr need to load before your HTML so naturally I include them in [_head.html.haml][_head]. To accommodate all other JavaScript files I use a [_scripts.html.haml][_scripts] partial located in the layouts folder.

#### What to Put in \<head>

If you haven't already noticed, there is a lot of commented out code in [_head.html.haml][<head>]. The reason for this is that there is a lot of things that you could put there, but don't. Obviously just put in what you need. I provide the bare minimum, but everything else, commented out, just in case you need it.

For an explanation on what this stuff is/does check out:

- [Helpful things to keep in your \<head>][Helpful Things]

##### The Title

You may have also noticed that I use the following snippet of code in [<head>][_head]:

    %title= content_for?(:title) ? yield(:title) : "XXX"

You can use whatever you would like, or nothing at all (although I wouldn't advise this), but please, whatever you do, DO NOT put the actual titles in your controllers. Text belongs in views and should stay there whenever humanly possible. With the above code I would add this to an individual page:

    - content_for :title do
      Some Cool Title

### Moving Forward

Before we move on to styles, there are a few things we need to consider when adding files and organizing code within our framework moving forward.

#### Using Partials

Partials help:

1.  Increase readability of your code
2.  Keep things organized
3.  Reuse code, i.e. keep things DRY
4.  Encapsulate logic, although helper methods are preferred

The key is not to go nuts and partial everything. If you do that, finding things can become a wild goose chase for backing coders trying to figure out where the markup they need lives.

#### Naming Conventions

When naming HTML files try to stay within REST conventions, i.e. index.html.haml, show.html.haml, etc.. When naming outside of REST be short and concise, use names that indicate what the files function or purpose are.

When naming HTML files separate words with underscores:

    a_great_file_name.html.haml

For partials, if there are several related to an individual file, group them by using the parent files name or abbreviation. If there is no parent file per se, use a common function or purpose to group the partials, for example:

    _tabs.html.haml
    _tabs_sub_nav.html.haml

#### IE 6

These days most people like to just forget about IE 6, it's old, "We're not supporting it, those users need to update!" ...But here's the problem, those users are still out there, even if they don't want to be. Have you ever been in your local public library and checked out the super old machines people have to use there? I sometimes go just to see what an application I'm working on will look like in older browsers.

So what I propose is a compromise for those of you who don't want to waste your time developing for IE 6; give those users notice and ability via [Chrome Frame][]. If you're using my [starter code][] you'll notice that I added to the [following partial][_chromeframe] to your layouts folder:

    /[if lt IE 7 ]
      %p.chromeframe
        Your browser is
        %em ancient!
        %a{:href => "http://browsehappy.com/"} Upgrade to a different browser or
        %a{:href => "http://www.google.com/chromeframe/?redirect=true"} install Google Chrome Frame to experience this site.

Here is the corresponding helper method:

    def chromeframe
      render :partial => 'layouts/chromeframe'
    end

There really are a lot of factors that could prevent the user from upgrading:

> As a web developer, you’re working with IT every day. Upgrading doesn’t worry you; it’s easy and everyone should do it. But are you neglecting to consider:
>
>- Large organizations and government departments. Those businesses may have 10-year IT plans. Desktops are locked-down and users can’t upgrade. Even when a company wants to move forward, migrating thousands of users is not quick, simple or inexpensive.
>- Windows XP users. One in four people use XP and that figure is higher for business users. Upgrading beyond IE8 is not an option.
>- You are not an average user. Most people do not understand IT. Many are terrified of it — or certainly worried they’ll break their PC. Migrating from something they know is a risk regardless of the benefits.

\- [Is Internet Explorer Development Really a Waste of Time?][Waste of Time]

I should mention that the author of this article follows up with a solution in his follow-up article:

- [How to Use Responsive Web Design to Support Old Browsers][RWD for IE]

### What We've Done

- We started this chapter by laying out the groundwork for your foundation markup.
- We then began to discuss what an application layout is, from the front end coder's viewpoint, and how it is organized in a Rails application.
- We then grabbed all of the starter code for this chapter, our foundation markup, and integrated into a new Rails application.
- Finally, we took a look at how this foundation markup is organized and briefly reviewed legacy browsers.

In the [next chapter][Foundation Styles], we will begin to set up our foundation styles.


[RoR Tutorial]:         http://ruby.railstutorial.org/book/ruby-on-rails-tutorial?version=3.2
[Gemfile]:              https://github.com/maxxiimo/base-haml/blob/master/Gemfile
[.gitignore]:           https://github.com/maxxiimo/base-haml/blob/master/.gitignore
[Turbo Sprockets]:      https://github.com/ndbroadbent/turbo-sprockets-rails3
[H5BP .gitignore]:      https://github.com/h5bp/html5-boilerplate/blob/master/.gitignore
[Tutorial .gitignore]:  http://ruby.railstutorial.org/chapters/beginning?version=3.2#code:gitignore]
                        "An augmented .gitignore file"
[Ignore files]:         http://help.github.com/ignore-files/
[.gitignore]:           https://github.com/github/gitignore
[The Rails View]:       http://pragprog.com/book/warv/the-rails-view
[starter code]:         https://github.com/maxxiimo/base-haml
[HTML5 Boilerplate]:    http://html5boilerplate.com/
[Unofficial Guide]:     http://designreviver.com/articles/an-unofficial-guide-to-the-html5-boilerplate/
[H5BP for Rails]:       http://railsapps.github.com/rails-html5-boilerplate.html
[application]:          https://github.com/maxxiimo/base-haml/blob/master/views/layouts/application.html.haml
[application_helper]:   https://github.com/maxxiimo/base-haml/blob/master/helpers/application_helper.rb
[_chromeframe]:         https://github.com/maxxiimo/base-haml/blob/master/views/layouts/_chromeframe.html.haml
[_head]:                https://github.com/maxxiimo/base-haml/blob/master/views/layouts/_head.html.haml
[_scripts]:             https://github.com/maxxiimo/base-haml/blob/master/views/layouts/_scripts.html.haml
[_logo]:                https://github.com/maxxiimo/base-haml/blob/master/views/shared/_logo.html.haml
[_navigation]:          https://github.com/maxxiimo/base-haml/blob/master/views/shared/_navigation.html.haml
[_footer]:              https://github.com/maxxiimo/base-haml/blob/master/views/shared/_footer.html.haml
[Foundation Styles]:    https://github.com/maxxiimo/the-front-end-manifesto/blob/master/foundation-styles.md      
[<head>]:               https://github.com/maxxiimo/base-haml/blob/master/views/layouts/_head.html.haml
[Unholy Rails]:         http://railsapps.github.com/rails-javascript-include-external.html?utm_source=rubyweekly&utm_medium=email
[Helpful Things]:       https://gist.github.com/1981339
[ARIA roles]:           http://www.w3.org/TR/wai-aria/roles#landmark_roles
[Chrome Frame]:         https://developers.google.com/chrome/chrome-frame/
[Waste of Time]:        http://www.sitepoint.com/is-internet-explorer-development-really-a-waste-of-time/             
[RWD for IE]:           http://www.sitepoint.com/support-old-browsers-responsive-web-design/

[Basic HTML]:           http://chrismaxwell.com/rails-views/assets/getting-started/base-html-no-styles.png