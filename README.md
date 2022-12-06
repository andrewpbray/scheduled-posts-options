# Scheduled Posts

This is a test repo to try out adding functionality that would allow

1. Propagation of data info from `_schedule.yml` file to docs on render
2. Scheduled publishing by setting `draft: false` for any docs with dates later than the present

The second item seems like it can be handled by writing a filter as described [here](https://github.com/quarto-dev/quarto-cli/discussions/3114). I can think of three approaches to doing 1).


#### 1. YML injection via .md file created in pre-render script

Write a pre-render script that parses `_schedule.yml` and creates a tiny `_publish-date.md` file next to each post that adds to the metadata via an include. This is quasi-implemented in Topic 2.

Downsides:
- feels kludgy
- generates a bunch of files, though this could be addressed by
  - adding files to .gitignore
  - keep files in dedicated include directory
  - create a post-render script to clean out that directory

#### 2. Reference _variables.yml in document YAML

Create a `_variables.yml` file that includes the publish dates for each post. That variable is then referenced in the yaml header of the post. This is implemented in Topic 3 (though apparently I've formatted the date field incorrectly).

A similar use case is discussed [here](https://github.com/quarto-dev/quarto-cli/discussions/2865).

Downsides:
- Requires tailoring the yaml of each doc to reference the appropriate date.
- Seems like a misuse of variables. These are meant to be shared across documents, right? Here they're document specific.

#### 3. Write a filter

Changing the meta data of a documents seems solidly within the functionality of a filter. The wrinkles that I see here:

1. The filter will need to reference information in `_schedule.yml`. Is there an example of a filter that does this that I could look at?
2. Right now, `_schedule.yml` links the publish date with the file / directory name of the post. I'm not sure how to reference that information in the filter. Would it make more sense to reference the `title` field in the yaml?