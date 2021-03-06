YET ANOTHER SIMPLE TOTP TOKEN GENERATOR IN PURE C
=======


This is a simple (re)implementation of the **TOTP Token generator** written in Pure C and following the 
references:

* [RFC6238](https://tools.ietf.org/html/rfc6238)
* [RFC4226](https://tools.ietf.org/html/rfc4226)

It doesn't expose any external api, you can just build it and use with the most popular provider
that implement the RFC specification.

BUILD FROM SRC
----
You can simply build it using the Makefile provided.
When you run **make** it generates a bin/ folder inside the root dir of the project containing the result of the compilation.


USAGE
---

    $ ./c_otp -h
    Usage ./bin/c_otp [-f fname] | [-b b32_secretkey] [-m mode] [-v]

According to the help of the command, you can have to mode:

* **direct**: it is used to run a totp calculation *on the fly*.

To do this simply run:

    $ c_otp -b <BASE32_SECRET>

and if the secret is a valid base32, it executes the decode and then performs the calculation.
If you want to make a test you can generate an example test VECTOR obtained as the following:

    $ echo "test" | base32 (ORSXG5A=)


* **provider**: this is the most interesting mode because you can define the list of your providers
in a *providerrc* file and pass it to the binary:

    $ c_otp -f providerrc -s

The -s flag inspiration is taken from the *slstatus suckless* project and it is used to print on the
**STDOUT** the array containing all the defined providers.
However, as the helper suggests, we can pass the `-m mode` option which can be used to provide a json
like format output, useful to interact with other kind of applications.
The accepted values are `0` and `1`, where `0` represents the default value if this option is not
passed; if `1` is passed as value, a json like format output is provided.

For instance running:

    $ ./c_otp -f providerrc -m 1

we can obtain the following output:

    {
            "providers": {
                    "protonmail": "123456",
                    "google": "123456",
                    "amazon": "123456",
            }
    }

and it can be parsed using tools like `jq`; in fact, we can filter by provider name and get just the
value running:

    $ ./c_otp -f providerrc -m 1 | jq '.providers.protonmail'

In the project tree a *providerrc.sample* is provided and it's used for testing purposes:

    #provider:secret ###THIS IS A COMMENT: it will be ignored ..
    protonmail:ORSXG5A=
    amazon:MVUGY3Y=
    google:ORSXG5A=
    #trello:ORSKGGGH5A=

You can just use it as a starting point example.
