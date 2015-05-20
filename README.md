# Redmine2 Cookbook

[![Build Status](https://secure.travis-ci.org/gortazar/redmine2-cookbook.png?branch=master)](http://travis-ci.org/aminin/redmine2-cookbook)
[![Dependencies](https://gemnasium.com/gortazar/redmine2-cookbook.png?branch=master)](https://gemnasium.com/aminin/redmine2-cookbook)

Installs Redmine v2, a Ruby on Rails ticket tracking and wiki tool

## Requirements

### Platform

Tested on ubuntu 12.04, 14.04

### cookbooks

* postgresql
* [rbenv](https://github.com/fnichol/chef-rbenv)
* [ruby_build](https://github.com/fnichol/chef-ruby_build)
* nginx
* runit
* database

## Attributes

| Key                                    | Type    | Description                      | Default                      |
|----------------------------------------|---------|----------------------------------|------------------------------|
| <tt>['redmine']['home']</tt>           | String  | Location for Redmine application | <tt>/home/redmine</tt>       |
| <tt>['redmine']['host']</tt>           | String  | Redmine Domain                   | <tt>redmine.example.com</tt> |
| <tt>['redmine']['user']</tt>           | String  | Owner of redmine files           | <tt>redmine</tt>             |
| <tt>['redmine']['ruby_version']</tt>   | String  | Redmine Ruby Version             | <tt>1.9.3-p484</tt>          |
| <tt>['redmine']['version']</tt>        | String  | Redmine version                  | <tt>2.6.1</tt>               |
| <tt>['redmine']['db']['type']</tt>     | String  | Type of redmine database         | <tt>postgresql</tt>          |
| <tt>['redmine']['db']['dbname']</tt>   | String  | Redmine DB name                  | <tt>redmine</tt>             |
| <tt>['redmine']['db']['username']</tt> | String  | Redmine DB user                  | <tt>redmine</tt>             |
| <tt>['redmine']['db']['hostname']</tt> | String  | Redmine DB host                  | <tt>localhost</tt>           |
| <tt>['redmine']['db']['password']</tt> | String  | Redmine DB password              | <tt>123456</tt>              |
| <tt>['redmine']['themes']</tt>         | Array   | Redmine extra themes             | <tt>empty</tt>               |
| <tt>['redmine']['plugins']</tt>        | Array   | Redmine plugins                  | <tt>empty</tt>               |

## Usage

To install via berkshelf add to your Berksfile the following lines

```
cookbook 'rbenv', git: 'https://github.com/aminin/chef-rbenv'
cookbook 'redmine2', git: 'https://github.com/aminin/redmine2-cookbook'
```

and run `berks install`

Configure your role/node e.g.:

```ruby
{
    nginx: {
        default_site_enabled: false # Disable default nginx site
    },
    redmine: {
        host: 'redmine.dev',
        db: {
            password: '<top-secret1>'
        },
        plugins: [
            {
                name: 'redmine_github_hook',
                type: 'git',
                source: 'https://github.com/koppen/redmine_github_hook.git'
            }
        ],
        themes: [
            {
                name: 'pixel-cookers',
                type: 'git',
                source: 'https://github.com/pixel-cookers/redmine-theme.git'
            }
        ],
        config: {
            email: {
                smtp_settings: {
                    address: smtp.yandex.ru
                    port: 465
                    ssl: true
                    enable_starttls_auto: true
                    domain: redmine.dev
                    authentication: :login
                    user_name: 'redmine@redmine.dev'
                    password: '<top-secret123>'
                }
            }
        }
    },
    postgresql: {
        password: {
            postgres: '<top-secret2>' # Need admin access to create redmine DB
        }
    },
    run_list: %w(recipe[postgresql::server] recipe[redmine2] recipe[redmine2::themes] recipe[redmine2::plugins])
}
```

## Runing tests

```
bundle exec rake foodcritic
bundle exec rake kitchen:all
```

## Contributing

1. Fork the repository on Github
2. Create a named feature branch (like `add_component_x`)
3. Write your change
4. Write tests for your change (if applicable)
5. Run the tests, ensuring they all pass
6. Submit a Pull Request using Github

## License and Authors

Authors: TODO: List authors
