% SHOOGLE(1)
% 
% Nov 2017

# NAME

shoogle - command-line access to the Google API

# SYNOPSIS

`shoogle` `-h`

`shoogle` `show` [`--debug-request-level`] [`--debug-response-level`] [&lt;API-PATH&gt;]

`shoogle` `execute` [`-c <CLIENT-SECRET-FILE>`] [`--credentials-file CREDENTIALS-FILE` | `--credentials-profile CREDENTIALS-PROFILE>`] [`-f <path>`] &lt;API-PATH&gt; &lt;JSON-FILE&gt;

# DESCRIPTION

**shoogle** is a CLI tool which provides access to Google API functions. It
can be used to discover interfaces and methods, and to execute them.


## Parameters

**-h**, **--help**
:    Print command-line help information.

**show**
:    List information about Google API Services, Resources, and methods. information.

**execute**
:    Execute a Google API method.

**JSON-FILE**
:    JSON-formatted file containing API request information. See JSON-FILE
     for details.

**API_PATH**
:     The path specification for a Google API method. See API-PATH for details.


### 'Show' Command Options
**--debug-request-level &lt;n&gt;**
:    A non-negative integer indicating the recursion level for information output for method request documentation.

**--debug-response-level &lt;n&gt;**
:    A non-negative integer indicating the recursion level for information output for method response documentation.


### 'Execute' Command Options
**-c &lt;CLIENT-SECRET-FILE&gt;**, **--client-secret-file &lt;CLIENT-SECRET-FILE&gt;**
:    A path to the client secret JSON file, to be used to start OAuth2 authentication. See CLIENT-SECRET-FILE for details.

**-f &lt;path&gt;**, **--media-file &lt;path&gt;**
:    File to use for media-related operations.

**--credentials-file &lt;CREDENTIALS-FILE&gt;**
:    Select a specific set of established OAuth2 credentials. See CREDENTIALS-FILE for details.

**--credentials-profile &lt;CREDENTIALS-PROFILE&gt;**
:    Select a profile for the OAuth2 credentials. See CREDENTIALS-PROFILE for details.


## JSON-FILE

The '`shoogle show`' command can show the request information needed for an API method. This file contains the required input parameters for a method to be executed, in that format.

A value of '`-`' indicates that the JSON information will be read from STDIN.

## API-PATH

The "API Path" for an API method is of the form "SERVICE:VERSION.RESOURCE.METHOD". 

For the '`show`' command, this parameter is optional, and may be truncated. For instance, if it is ommitted, a list of Services is shown. If the method name is left off, a list of methods is shown. For a full path, detail in the method inputs and outputs is shown.

For the '`execute`' command, this defines the method to be called.

## CLIENT-SECRET-FILE

The Client Secret JSON file contains user information for authorization. It can be downloaded from the Google Console OAuth 2.0 Client ID page. The client IDs are listed on the Credentials page (*https://console.cloud.google.com/apis/credentials*).

## CREDENTIALS-FILE

The CREDENTIALS-FILE contains the authorization tokens generated from a successful OAuth 2.0 executed transaction. It allows authentication to succeed on subsequent calls without interaction by the
user.

By default, this file is stored in *~/.shoogle/credentials/&lt;profile&gt;/*. This option overrides
that path.

Typically, this option is not requred.

## CREDENTIALS-PROFILE

**shoogle** aggregates CREDENTIALS-FILEs by profile. This option allows the profile name to be 
specified. The default is "default".

Typically, this option is not required.

# EXAMPLES

List services:

    shoogle show

List resources for a service:

    shoogle show urlshortener:v1.

List methods for a resource:

    shoogle show urlshortener:v1.url

List parameters and results for a method:

    shoogle show urlshortener:v1.url.list

Call a method:

    echo '{}' | shoogle execute -c <secrets.json> urlshortener:v1.url.list -

*jq* is a command-line JSON builder/parser. This example shows how to upload a video from a JSON template and extract the ID from the response:


```shell
$ cat upload-video.template.json
{
  "part": "snippet",
  "body": {
    "snippet": {
      "title": $title,
      "description": $description
    }
  }
}
```

```shell
$ jq -n -f upload-video.template.json --arg title "Chess" --arg description "Norway Chess" |
    shoogle execute -c your_client_id.json youtube:v3.videos.insert - -f chess.mp4 |
    jq -r '.id'
wUArz2nPGqA
```

# SEE ALSO

**jq(1)**

