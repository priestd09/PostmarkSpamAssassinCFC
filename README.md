### Requirements

ColdFusion 8 or above (i.e. JSON functions are supported).

### Usage

Create an instance of the CFC. You might like to cache this in a persistent scope, e.g. the application scope.

`spamchecker = createObject("component", "PostmarkSpamAssassin");`

Then pass the email to be checked to the CFC methods:

`spamchecker.getScore(email)` - returns the spam score

`spamchecker.getReport(email)` - returns the detailed SpamAssassin report

You can also access the full JSON response:

`spamchecker.doSpamCheck(email)`

### Error Handling

All errors are thrown for you to catch:

 - if the remote call fails or times out
 - if the remote call returns an HTTP error code
 - if SpamAssassin returns an error (the message of the exception will be the error)
