# Rusoto Core

<a href="https://github.com/rusoto/rusoto/actions?query=workflow%3A%22Build+and+test%22"><img src="https://github.com/rusoto/rusoto/workflows/Build%20and%20test/badge.svg"></a>
<a href="https://docs.rs/rusoto_core" title="API Docs"><img src="https://img.shields.io/badge/API-docs-blue.svg" alt="api-docs-badge"></img></a>
<a href="https://crates.io/crates/rusoto_core" title="Crates.io"><img src="https://img.shields.io/crates/v/rusoto_core.svg" alt="crates-io"></img></a>
<a href="#license" title="License: MIT"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="license-badge"></img></a>
<a href="https://deps.rs/repo/github/rusoto/rusoto" title="Dependency Status"><img src="https://deps.rs/repo/github/rusoto/rusoto/status.svg" alt="dependency-status-badge"></img></a>
<a href="https://discordapp.com/invite/WMJ4DWp"><img src="https://img.shields.io/discord/670751965273391124"></img></a>

**Rusoto is an AWS SDK for Rust**

---

You may be looking for:

* [An overview of Rusoto][rusoto-overview]
* [AWS services supported by Rusoto][supported-aws-services]
* [API documentation][api-documentation]
* [Getting help with Rusoto][rusoto-help]

## Installation

Rusoto is available on [crates.io](https://crates.io/crates/rusoto_core).
To use Rusoto in your Rust program built with Cargo, add it as a dependency and `rusoto_$SERVICENAME` for any supported AWS service you want to use.

For example, to include only S3 and SQS:

``` toml
[dependencies]
rusoto_core = "0.47.0"
rusoto_sqs = "0.47.0"
rusoto_s3 = "0.47.0"
```

## Migration notes

Breaking changes and migration details are documented at [https://rusoto.org/migrations.html](https://rusoto.org/migrations.html).

Note that from v0.43.0 onward, Rusoto uses Rust's `std::future::Future`, and the Tokio 0.2 ecosystem.

## Usage

Rusoto has a crate for each AWS service, containing Rust types for that service's API.
A full list of these services can be found [here][supported-aws-services].
All other public types are reexported to the crate root.
Consult the rustdoc documentation for full details by running `cargo doc` or visiting the online [documentation](https://docs.rs/rusoto_core) for the latest crates.io release.

A simple example of using Rusoto's DynamoDB API to list the names of all tables in a database:

```rust,no_run
use rusoto_core::Region;
use rusoto_dynamodb::{DynamoDb, DynamoDbClient, ListTablesInput};

#[tokio::main]
async fn main() {
    let client = DynamoDbClient::new(Region::UsEast1);
    let list_tables_input: ListTablesInput = Default::default();

    match client.list_tables(list_tables_input).await {
        Ok(output) => match output.table_names {
            Some(table_name_list) => {
                println!("Tables in database:");

                for table_name in table_name_list {
                    println!("{}", table_name);
                }
            }
            None => println!("No tables in database!"),
        },
        Err(error) => {
            println!("Error: {:?}", error);
        }
    }
}
```

### Usage with rustls

If you do not want to use OpenSSL, you can replace it with rustls by editing your Cargo.toml:

``` toml
[dependencies]
rusoto_core = { version="0.47.0", default_features=false, features=["rustls"] }
rusoto_sqs = { version="0.47.0", default_features=false, features=["rustls"] }
rusoto_s3 = { version="0.47.0", default_features=false, features=["rustls"] }
```

### Credentials

For more information on Rusoto's use of AWS credentials such as priority and refreshing, see [AWS Credentials][aws-credentials].

## Semantic versioning

Rusoto complies with [semantic versioning 2.0.0](http://semver.org/).
Until reaching 1.0.0 the API is to be considered unstable.
See [Cargo.toml](Cargo.toml) or [rusoto on crates.io](https://crates.io/crates/rusoto_core) for current version.

## Releases

Information on release schedules and procedures are in [RELEASING][releasing].

## Contributing

See [CONTRIBUTING][contribution].

## Supported OSs and Rust versions

Linux, OSX and Windows are supported and tested via Azure Pipelines and Appveyor.

Rust stable, beta and nightly are supported.

## License

Rusoto is distributed under the terms of the MIT license.

See [LICENSE][license] for details.

[api-documentation]: https://docs.rs/rusoto_core "API documentation"
[license]: https://github.com/rusoto/rusoto/blob/master/LICENSE "MIT License"
[rusoto-help]: https://www.rusoto.org/help.html "Getting help with Rusoto"
[rusoto-overview]: https://www.rusoto.org/ "Rusoto overview"
[supported-aws-services]: https://www.rusoto.org/supported-aws-services.html "List of AWS services supported by Rusoto"
[aws-credentials]: https://github.com/rusoto/rusoto/blob/master/AWS-CREDENTIALS.md "AWS Credentials"
[releasing]: https://github.com/rusoto/rusoto/blob/master/RELEASING.md "Releasing Rusoto"
[contribution]: https://github.com/rusoto/rusoto/blob/master/CONTRIBUTING.md "Contributing to Rusoto"
