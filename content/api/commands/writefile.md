---
title: writeFile
---

Write to a file with the specified contents.

## Syntax

```javascript
cy.writeFile(filePath, contents)
cy.writeFile(filePath, contents, encoding)
cy.writeFile(filePath, contents, options)
```

### Usage

**<Icon name="check-circle" color="green"></Icon> Correct Usage**

```javascript
cy.writeFile('menu.json')
```

### Arguments

**<Icon name="angle-right"></Icon> filePath** ***(String)***

A path to a file within the project root (the directory that contains the default `cypress.json`).

**<Icon name="angle-right"></Icon> contents** ***(String, Array, or Object)***

The contents to be written to the file.

**<Icon name="angle-right"></Icon> encoding**  ***(String)***

The encoding to be used when writing to the file. The following encodings are supported:

* `ascii`
* `base64`
* `binary`
* `hex`
* `latin1`
* `utf8`
* `utf-8`
* `ucs2`
* `ucs-2`
* `utf16le`
* `utf-16le`

**<Icon name="angle-right"></Icon> options**  ***(Object)***

Pass in an options object to change the default behavior of `cy.writeFile()`.

Option | Default | Description
--- | --- | ---
`log` | `true` | Displays the command in the [Command log](/guides/core-concepts/test-runner#Command-Log)
`flag` | `w` | File system flag as used with [`fs.writeFile`](https://nodejs.org/api/fs.html#fs_file_system_flags)
`encoding` | `utf8` | The encoding to be used when writing to the file

<Alert type="info">


To use encoding with other options, have your options object be your third parameter and include encoding there. This is the same behavior as [`fs.writeFile`](https://nodejs.org/api/fs.html#fs_fs_writefile_file_data_options_callback).

</Alert>

### Yields [<Icon name="question-circle"/>](introduction-to-cypress#Subject-Management)

<List><li>`cy.writeFile()` 'yields the value of the <code>contents</code> argument' </li></List>

## Examples

### Text

#### Write some text to a `txt` file

If the path to the file does not exist, the file and its path will be created. If the file already exists, it will be over-written.

```javascript
cy.writeFile('path/to/message.txt', 'Hello World')
cy.readFile('path/to/message.txt').then((text) => {
  expect(text).to.equal('Hello World') // true
})
```

`{projectRoot}/path/to/message.txt` will be created with the following contents:

```text
 "Hello World"
```

### JSON

#### Write JSON to a file

JavaScript arrays and objects are stringified and formatted into text.

```javascript
cy.writeFile('path/to/data.json', { name: 'Eliza', email: 'eliza@example.com' })
cy.readFile('path/to/data.json').then((user) => {
  expect(user.name).to.equal('Eliza') // true
})
```

`{projectRoot}/path/to/data.json` will be created with the following contents:

```json
{
  "name": "Eliza",
  "email": "eliza@example.com"
}
```

#### Write response data to a fixture file

```javascript
cy.request('https://jsonplaceholder.typicode.com/users').then((response) => {
  cy.writeFile('cypress/fixtures/users.json', response.body)
})

// our fixture file is now generated and can be used
cy.fixture('users').then((users) => {
  expect(users[0].name).to.exist
})
```

### Encoding

#### Specify the encoding as a String

```javascript
cy.writeFile('path/to/ascii.txt', 'Hello World', 'ascii'))
```

`{projectRoot}/path/to/message.txt` will be created with the following contents:

```text
Hello World
```

#### Specify the encoding as part of the options object

```javascript
cy.writeFile('path/to/ascii.txt', 'Hello World', { encoding: 'ascii', flag: 'a+' })
```

### Flags

#### Append contents to the end of a file

```javascript
cy.writeFile('path/to/message.txt', 'Hello World', { flag: 'a+' })
```

## Rules

### Requirements [<Icon name="question-circle"/>](introduction-to-cypress#Chains-of-Commands)

<List><li>`cy.writeFile()` requires being chained off of `cy`.</li><li>`cy.writeFile()` requires the file be successfully written to disk. Anything preventing this such as OS permission issues will cause it to fail.</li></List>

### Assertions [<Icon name="question-circle"/>](introduction-to-cypress#Assertions)

<List><li>`cy.writeFile` will only run assertions you have chained once, and will not [retry](/guides/core-concepts/retry-ability).</li></List>

### Timeouts [<Icon name="question-circle"/>](introduction-to-cypress#Timeouts)

<List><li>`cy.writeFile()` should never time out.</li><li><Alert type="warning">


Because `cy.writeFile()` is asynchronous it is technically possible for there to be a timeout while talking to the internal Cypress automation APIs. But for practical purposes it should never happen.

</Alert></li></List>

## Command Log

***Write an array to a file***

```javascript
cy.writeFile('info.log', ['foo', 'bar', 'baz'])
```

The command above will display in the Command Log as:

<DocsImage src="/img/api/writefile/write-data-to-system-file-for-testing.png" alt="Command Log writeFile" ></DocsImage>

When clicking on the `writeFile` command within the command log, the console outputs the following:

<DocsImage src="/img/api/writefile/console-log-shows-contents-written-to-file.png" alt="Console Log writeFile" ></DocsImage>

## History

Version | Changes
--- | ---
[4.0.0](/guides/references/changelog#4-0-0) | `cy.writeFile()` now yields `null` instead of `contents`
[3.1.1](/guides/references/changelog#3-1-1) | Added `flag` option and appending with `a+`
[1.0.0](/guides/references/changelog#1.0.0) | `cy.writeFile()` command added

## See also

- [`cy.readFile()`](/api/commands/readfile)
