# Implementation requirements for the CVE User Registry

Define the purpose of the repo

# Function of the repository

- Maintain a listing of all contributors of CVE data on GitHub to include CVE Numbering Authorities and authorized data contributors
- Maintain metadata about each contributor
    - Provide identifiers for each contributor
    - Associate keying material with each contributor to be used to validate signed pull requests
    - Document data scope for each contributor in a way that will support authentication and validation of a given contribution
- As an original contributor of a CVE entry, describe policies for who may modify the content of the contribution and in what way to provided for automated authentication and validation of a given modification to an existing entry


# Workflows

## Authenticating a contributor

The CVE User Registry will be used by other services as a source of authentication and authorization information. The following is a description of the authentication workflow. The Authorization workflows will be defined based on the business rules of the authorizing service.

1. TBD

## Authenticating data signed by a contributor

When the infrastructure receives data from a contributor, there is a need to validate the source of this data. A public key stored in the CVE User Repository will be used for this purpose.

1. TBD 

## Encrypting confidential data using the contributors public key

For communications related to sensitive CVE operations or vulnerability information there is a need to secure communications between the CVE infrastructure and individual participants. A public key for the contributor, stored in the CVE User Repository will be used for this purpose. The following workflow presents how this will be done.

1. TBD 

## Adding a new contributor entry to the repository

1. Stakeholder provides user data.
1. Validate that the data is well-formed and complete
1. Determine if stakeholder is authorized to add the record.
1. Check for existing record, if exists report failure.
1. Create new record.
1. Publish record to repository.

## Modifying an existing contributor entry

1. Stakeholder provides user data and identifies record to update.
1. Validate that the data is well-formed and complete
1. Check for existing record, if none exists report failure.
1. Determine if stakeholder is authorized to modify the record.
1. Update record with new data.
1. Publish updated record to repository.

## Removing a contributor entry

1. Stakeholder identifies record to delete.
1. Check for existing record, if none exists report failure.
1. Determine if stakeholder is authorized to delete the record.
1. Delete identified record.

# Requirements for Implementation 

## Data requirements

- Keep revision history showing changes to the record
- Identify the current revision level of each entry (e.g., monotonically increasing number)
- Provide a means of accessing only the current information; also a historic version

> Based on CVE CNA JSON File Format 0.3 from George Theall

- data_type: This attribute is a string that identifies the type of data in the file. It is REQUIRED and MUST equal "CVE CNA".
- data_version: This attribute is a string that identifies the version of the schema used by the file so that tools can more easily parse the data in CNA JSON files. It is REQUIRED.
- data_changelog: This attribute is an array of one or more objects presenting changes to the file. It is RECOMMENDED.

  Each object has the following properties :

  - description: This attribute is an array with one or more language-string objects providing a description of the change. It is REQUIRED. It is RECOMMENDED that there be an English language-string object.
  - timestamp: This attribute is a string representing when the change was made, following the date/time format in RFC 3339. It is REQUIRED. It is RECOMMENDED that the value include a time zone designator.

  - name: This attribute is an object that names the CNA.

It is REQUIRED.

It has the following properties :

aliases
This attribute is an array with one or more strings by which the CNA can be referenced, such as in a parent's list of sub-CNAs.

It is OPTIONAL.

long_name
This attribute is an array with one or more language-string objects presenting the full
name for the CNA.

It is REQUIRED.

It is RECOMMENDED that there be an English language-string object.

uuid
This attribute is a string with a UUID that uniquely identifies the CNA.

It is REQUIRED.

contact
This attribute is an object that provides various points of contact for the CNA.

It is REQUIRED.

It has the following properties :

general
This attribute is an object providing general points of contact for the CNA's organization.

It is OPTIONAL.

It has the following properties :

email
This attribute is an array with one or more objects about an email point of contact.

It is OPTIONAL.

Each object has the following properties :

address
This attribute is a string with an email address.

It is REQUIRED.

label
This attribute is a language-string object describing the email address.

It is OPTIONAL.

It is RECOMMENDED that, if used, there be an English language-string object.

web
This attribute is an array providing information about a web point of contact.

It is OPTIONAL.

address
This attribute is a string with a URL.

It is REQUIRED.

label
This attribute is a language-string object describing the URL.

It is OPTIONAL.

It is RECOMMENDED that, if used, there be an English language-string object.

security
This attribute is an object providing security-related points of contact for the CNA's organization.

It is REQUIRED.

It has the following properties :

email
This attribute is an array with one or more objects about an email point of contact.

It is REQUIRED.

Each object has the following properties :

address
This attribute is a string with an email address.

It is REQUIRED.

label
This attribute is a language-string object describing the email address.

It is OPTIONAL.

It is RECOMMENDED that, if used, there be an English language-string object.

web
This attribute is an array providing information about a web point of contact.

It is OPTIONAL.

address
This attribute is a string with a URL.

It is REQUIRED.

label
This attribute is a language-string object describing the URL.

It is OPTIONAL.

It is RECOMMENDED that, if used, there be an English language-string object.

scope
This attribute is an array of one or more language-string objects presenting the CNA's scope.

It is REQUIRED.

It is RECOMMENDED that there be an English language-string object.

block_assignments
This attribute is an object with information about id blocks reserved by the CNA.

It is RECOMMENDED to support automation in the cvelist repo.

It has the following properties :

auto_populate
This attribute specifies whether information about id blocks should be managed by the primary or parent CNA.

It is REQUIRED.

blocks
This attribute is an array of one or more objects with information about id blocks for a specific year.

It MUST NOT be present if the auto_populate attribute is false. Otherwise, the primary or parent CNA MUST handle updating the information about id blocks reserved by that CNA.

Each object in the blocks array has the following properties :

year
This attribute is an integer corresponding to the year of the block.

It is REQUIRED.

id_blocks
This attribute is an array with one or more objects specifying the id blocks reserved for the associated year.

It is REQUIRED.

Each object in the id_blocks array has the following properties :

start
This attribute is an integer specifying the start of an id block.

It is REQUIRED. And the value MUST be greater than 0.

end
This attribute is an integer specifying the end of an id block.

It is REQUIRED. And the value MUST be equal to or greater than the start value.

committers
This attribute is an array of one or more objects about Github accounts that may operate on behalf of the CNA.

It is RECOMMENDED to support automation in the cvelist repo.

Each object in the committers array has the following properties :

user_name
This attribute is a string specifying a Github account name of the committer.

It is REQUIRED.

gpg_keys
This attribute is a string holding the GPG keyring (ASCII armored) of the committer.

It is OPTIONAL but RECOMMENDED.

email
This attribute is a string holding the email address of the committer..

It is OPTIONAL but RECOMMENDED.

fullname
This attribute is a string with the full name of the committer.

It is OPTIONAL.

parent-cnas
This attribute is an array with one or more parent-CNAs, specified by their associated short (abbreviated) name.

It is OPTIONAL.

sub-cnas
This attribute is an array with one or more sub-CNAs, specified by their associated short (abbreviated) name.

It is OPTIONAL.

types
This attribute is an array with one or more CNA types, as listed among the list of participating CNAs.

It is REQUIRED.

## Security requirements

- For a stakeholder to be authorized to add, modify, or delete a record, they must be the record owner, a parent or ancestor rCNA, or other administrative authority that has been given access.
