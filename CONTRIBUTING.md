# Contributing

Feedback and contributions are very welcome!  For specific proposals, please provide them as issues or pull requests via our [GitHub site](https://github.com/linuxfoundation/cii-best-practices-badge).  Pull requests are especially appreciated!  For general dicussion, feel free to use the [cii-badges mailing list](https://lists.coreinfrastructure.org/mailman/listinfo/cii-badges).

If you just want to propose or discuss changes to the criteria, the first step is proposing changes to the criteria text, which is in the file [criteria.md](doc/criteria.md).  The "doc/" directory has information you may find helpful, including [other.md](doc/other.md) and [background.md](doc/background.md).

To make changes to the "BadgeApp" web application that implements the criteria, you may find the following helpful; [INSTALL.md](doc/INSTALL.md) (installation information) and [implementation.md](doc/implementation.md) (implementation infomrmation).

The web application is written in Ruby on Rails.  Please follow the [community Ruby style guide](https://github.com/bbatsov/ruby-style-guide) when writing code, e.g., use 2-space indents in Ruby.

SPECIAL RULE FOR RUBY: In general, prefer the String operations that do not have side-effects (e.g., "+", "sub", or "gsub"), and consider freezing strings.  Do *not* modify a String in-place (e.g., using "<<", "sub!", or "gsub!") until you have applied ".dup" to it.  There are current plans that [Ruby 3's strings will be immutable](https://twitter.com/yukihiro_matz/status/634386185507311616).  See [issue 11473](https://bugs.ruby-lang.org/issues/11473) for more.  One proposal is to allow "dup" to produce a slightly different object (a mutable version of String), and since "dup" is already permitted in the language, this provides a simple backwards-compatible way for us to indicate that String is mutable in this case.  If you want to build a string using append, do this:

~~~~ruby
"".dup << 'Hello, ' << 'World'
~~~~

There is a small amount of application-specific Javascript.  This is written in Javascript, not CoffeeScript; it's only a small amount of Javascript, and far more people know basic Javascript instead of CoffeeScript.

All markdown files use the .md extension and use [GitHub-flavored markdown](https://help.github.com/articles/github-flavored-markdown/), *not* CommonMark or the original Markdown.  In particular, [newlines in paragraph-like content are considered as real line breaks](https://help.github.com/articles/writing-on-github/) unlike the original Markdown and CommonMark formats.

When adding or changing functionality, please include new tests for them as part of your contribution.  The system uses minitest.

Before submitting changes, please run "rake" (no options) to look for problems, and fix the problems found.  In some cases it's okay to fix them by disabling the warning in that particular place, but be careful; it's often better to make a real change, even if it doesn't matter in that particular case. The specific list of tools run by default is listed in [default.rake](lib/tasks/default.rake).  Currently these include at least the following:

* bundle - use bundle to check dependencies ("bundle check || bundle install")
* "rake bundle_audit" - check for vulnerable dependencies
* "rake test" - runs the test suite
* "rake markdownlint" - runs markdownlint (check for errors in markdown text)
* "rake rubocop" - runs Rubocop, which checks code style against the [community Ruby style guide](https://github.com/bbatsov/ruby-style-guide)
* "rake rails_best_practices" - check against rails best practices using the gem [rails_best_practices](http://rails-bestpractices.com/)
* "rake brakeman" - runs Brakeman, which is a static source code analyzer
  to look for Ruby on Rails security vulnerabilities
