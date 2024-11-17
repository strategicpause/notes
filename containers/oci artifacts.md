### Notes
* OCI Image Manfiest Components
 * `schemaVersion` - v2 is the current format. v1 is the format introduced by the original Docker daemon and is now deprecated. 
 * `mediaType` - Manifest MIME type.
 * `config` - References configuration objects for a container. 
 * `layers` - An ordered list starting from the base layer. Includes a mediaType which indicates what kind of data is contained in a blob.
* OCI Artifacts
 * The OCI Artifacts initiative set out to make the registry format a bit more generic to allow users to store and distribute arbitrary files.
 * Essentially it's not a container image which is stored in a container registry that has a custom mediaType field.
 * An opinionated way to leverage OCI Registries for arbitrary artifacts without masquerading them as container images.

### Questions
* What is an OCI Registry?
	* An implementation of the [OCI distribution specification](https://github.com/opencontainers/distribution-spec/blob/main/spec.md).
* What is content address storage?
 * A way to store information so it can be retrieve based on its content, not its name or location.
 * Work by passing the content of the file through a cryptographic hash function to generate a unique key which is referred to as the content address.
* What is an OCI content descriptor? 
 * Includes the type of content, the content identifier (digest), and the byte-size of the raw content. Optionally it includes the type of artifact it is describing.
* What are annotations?
 * Annotations in OCI artifacts offer a means of adding metadata to various components include the Image Manifest, Image Index, and Image Layers.
* What is the Image Manifest?
* What is the Image Index?
* What are Image Layers?
* What is ORAS?
	* It works like Docker in that it allows you to push (upload) and pull (download) things to and from an OCI registry.
	* The de facto tool for working with OCI artifacts.

	
## Understanding OCI Artifacts
### OCI Specifications
OCI, or [Open Container Initiative](https://opencontainers.org/), is a Linux Foundation project dedicated to managing specificatiosn and projects related to the storage ([Image Spec](https://github.com/opencontainers/image-spec/blob/main/config.md)), distribution ([Distribution Spec](https://github.com/opencontainers/distribution-spec/blob/main/spec.md)), and the execution ([Runtime Spec](https://github.com/opencontainers/runtime-spec/blob/main/spec.md)) of container images. 

### OCI Registry

A container registry (ie: [ECR](https://aws.amazon.com/ecr/)), or OCI registry is an implementation of the OCI distribution spec. While registries are used to distribute container images, the standard itself is agnostic of content types. 

OCI registries offer a content-addressable. API, or a way to refer to files in a manner that assures their authenticity and integrity. For example, files are addressed by a digest which is generated by hashing the contents of the file via sha256.

### OCI Artifacts
Consists of an Image Manifest, or Image Index, which points to other OCI artifacts. These artifacts can be stored and accessed via content addressable storage like a registry.


An **OCI artifact** refer to any object stored in a container registry which are not container images whereas An OCI Artifact (note the capital A) refers to an OCI compliant image which includes custom `mediaType` values in the [image manfiest](https://github.com/opencontainers/image-spec/blob/main/manifest.md) to refer to the the artifacts being stored.

are a way of using an OCI registry to store arbitrary files, in a manner which is compliant with OCI.