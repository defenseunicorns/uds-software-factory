# 1. Store In-Toto Attestations in Archivista

Date: 2024-07-09

## Status

Accepted

## Context

During execution of Software Factory pipelines, we will be generating [in-toto attestations](https://github.com/in-toto/attestation) using [Witness](https://github.com/in-toto/witness) for the artifacts that are produced. We need a location to store these attestations so that they can later be retrieved and used to verify an artifact against policies.

## Decision

We will use [Archivista](https://github.com/in-toto/archivista) to store the attestations. Archivista is also an in-toto project and integrates easily with Witness to store, retrieve, and verify attestations. 

In addition, Archivista maintains a graph of metadata for the attestations, allowing attestations to be associated to all build artifacts. All that is needed to run a verification is an artifact and the Witness CLI, and the other data will be automatically retrieved from Archivista.

Policies can also be stored in Archivista, though this feature set isn't as mature yet.

## Alternatives considered

We could configure Witness to produce json files to the filesystem and store the files in a file storage solution ourselves. However, we would have to maintain an association between all the build artifacts and the correct files, and be able to retrieve the correct attestation file for the correct artifact at verification time. Archivista handles this for us and is lightweight and already in the in-toto family of tools.

## Consequences

We will need to create and maintain a new uds package for Archivista: [uds-package-archivista](https://github.com/defenseunicorns/uds-package-archivista).

Archivista stores the attestations in an S3 compatible blob storage and the metadata in an RDBMS (MySQL or Postgres). We will need to test backup and restore mechanisms for both of these.
