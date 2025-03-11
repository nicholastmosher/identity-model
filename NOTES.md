# March 10 2025

- I'm thinking more about how this could potentially be described as the foundations for
  a peer-to-peer browser. A browser is a program, and it performs tasks such as rendering
  content to a display, handling user input, performing network calls to fetch content,
  and storing some data locally.

- Important to have a DSL that helps to express the intentions of data and applications,
  and probably an interface hand-crafted for communicating key concepts to users.
  Ultimately I think I need to create a very simple domain model that would be easy to
  explain to average people. Find the right words to tell them that they deserve to feel
  confident that their data is kept private when they expect, and give them a clear mental
  model for how applications are allowed to ask to be a guest in your home but that they
  must prove to you that they are trustworthy by showing exactly how they're planning to
  use your data.

- Idea: A visualization that is a timeline? A scrolling feed, but with content backed by
  Willow/Meadowcap capabilities and displayed in a peer to peer programmable environment.
  Programmable environment would mean that not only could you post content, but also
  develop applications that can render a view onto your feed, such as games or documents
  or whatever else. Activity would be annotated with markings that clearly show which
  of your namespaces or subspaces data is coming from.

- Less feasible but fun idea: Imagine the timeline feed is a 3D cylinder of
  unbounded length, and the cross-section is a circular index of the data that
  was touched at a shown time. At a given range in the timeline, render planes that
  pass from the radius of the cylinder, like the pages of a notebook. Wedges around a
  span of the cylinder represent namespaces or subspaces that witnessed events at that
  time, such as reads and writes.

- Considering the global contexts pattern archetype such as seen with Bevy, GPUI, etc.,
  I wonder if it's possible to create an interface (DSL) that exposes the same application-
  building tools to developers working with both static and dynamic plugins. For example,
  by having a well-defined data model (e.g. something backed by Willow), we could create
  a programmatic DSL (think Bevy) that exposes a global-context-builder type of pattern
  but which is equally capable both inside and outside a WASM boundary. Capabilities may
  be able to sign WASM modules to enable sending functions to peers who have granted you
  the ability to do so.

# March 5 2025

- Create an event log for the purpose of delivering a security report
  at the beginning of each day. The app would deliver one notification
  each day, and it should be a summary of all peers that have participated
  in a backup of your peer's designated replicate-me data. The app presents
  a view that's a timeline of confirmed synchronization events with your
  trusted replication circle. This gives you a daily feed of confirming
  backups have been completed successfully. The objective would be to
  get people in the habit of being backup-conscious, because a genuine
  alternative to cloud software means that we'll need to set new
  expectations around digital security and data protection. I believe
  many people use cloud backup services because there's no better way yet
  to protect their data with such ease and confidence.

- Replicate-me data would be like albums or other directories of data that
  you ask trusted peers to keep a copy of for you. With a given peer, exchange
  an agreed upon amount of disk space to dedicate to storing that peer's data,
  their replicate-me data.

# March 5, 2025

- Choosing a concrete application as a basis for sketching the requirements for identity, data ownership
  and control, and access patterns. Using a “shared photo albums” app as the first use-case/case-study.
- Requirements for a photo sharing application:
  - Need some way to grant specific devices access to specific subsets of photos
  - Need a portable and ideally agnostic/extensible model for identities and data ownership
  - Data model needs to express the ability to employ redundancy among a trusted pool of devices or members
  - Distinguish the notion of “you can back up my data but not read it” vs “I’m sharing data that you can
    also read and/or write”
  - Potentially need a multi-level authority scheme, so for example ownership of a photo is expressed
    distinctly from who (group of participants) is allowed to read/write. “Writes” here may refer to the
    ability to reshare with other groups or to publish derivations.
  - A user should be able to create distinct profiles that may be used to participate in different spaces.
    It should not be possible to infer that one profile is controlled by the same person/key/authority as
    another profile. Motivation: have one profile for family spaces and another unassociated profile for
    other spaces, they should not be externally identifiable as being under the same control.
  - Some notion of “spaces” needs to be first-class, this would put access control front-and center, any
    application participating in a “space” with multiple participants would highlight that the data in that
    space is shared. May need some way to add or remove people from a group which may involve some dance with
    cryptographic keys rolling or such.
  - It should be possible to have a photo in a private space and share it to a public space. This may present
    as a capability granted from one space to another.
  - Transitive vs non-transitive capabilities, such as the capability for somebody to “hold” an encrypted
    backup of some of my data, which they may subsequently delegate to others to increase replication. Or
    non-transitive such as read access.
  - Levels of access: Read (can view file contents), Hold (knows the file exists and may replicate but not
    view the data), or Hidden (does not know the data exists at all).
  - Example: I share a folder of photos or documents with a group. The underlying operation would be to create
    a shared access key/identity for the group, then sign a capability for the chosen files that grants that
    group the ability to read/hold/etc. that data.

Next steps:

- Attempt to instantiate the Willow protocol and capabilities to express operations to fulfill the needs
  of a photo sharing app
- Think about acceptance tests for various scenarios

# Feb 6 2025

Project Kickoff!

- In our kickoff meeting today, we talked about various peer-to-peer technologies and
  compared and contrasted some of them with Ditto. I tried to express my current
  understanding of some peer-to-peer technologies I’ve read about such as libp2p,
  Willow, Automerge, etc., with the goal of reaching a common ground and understanding
  where our starting point will be and what my knowns, known-unknowns, and unknown-unknowns are.
- By next week, I want to put together a rough project proposal that we can refine together
  and create goals for outcomes by the end of the mentorship program. Ideally this will culminate
  in something that can be demoed and exhibit the learning outcomes.

Project Brainstorming/Ideas

- A project idea that I’ve been trying to define in my mind for awhile is a kind of
  framework for applications that puts users in control of their data. Similar to a
  browser which acts as an interface for users to navigate content that’s hosted on
  other peoples’ (corporations) machines, this project would consist of an interface
  for managing your own data on devices you control.
- This might be a many-stage project, but I have an idea of some of the components that
  might be required. The first component needs to be an identity system that feels as simple
  as any web account but offers strong cryptographic tools for constructing trusted groups
  of people, bots, and applications. The domain language of this system should be simple
  enough that it can be understood by average folks, but also be backed by a strong trust
  model. I’m currently considering using Willow (a data model) and Meadowcap (a capability
  system) as the basis for defining the primitives of this system. Willow and Meadowcap
  provide a versatile key-value framework which can leverage public-key cryptography to
  assign data ownership to identities, and for those identities to grant capabilities (a
  sort of controlled access to subsets of data). Delightfully, Willow is defined as a higher-
  order protocol, meaning that it’s own primitives are generic so they may be instantiated
  with particular choices of solutions for identifiers, indexing/addressing, cryptography, etc.
- I think what I want is to create a Willow module that defines an identity management system
  that is generic over different types of identity. Willow’s data model separates data by
  namespace, subspace, and paths. Namespaces and subspaces may be used for access control
  by using public keys and assigning ownership or control via capabilities to the holders
  of the corresponding private keys.
- Here’s the main problem scenario I haven’t managed to think my way through regarding
  identities. To illustrate, I’ll quickly describe a little-known app my family uses called
  band.us. It’s like a tiny little social app with a post feed, calendar, and chat, but the
  pitch for it is among a small tight group, like a marching band. I imagine something like
  this, where you can create a space that is a shared home for a particular set of shared data,
  like the family calendar. The group would need a cryptographic representation, so that each
  participant would be using a secure client to prove their right to access that pool of private
  data. However, I would like it to be easy to have a multi-identity paradigm, so that you can
  have identities that are under your control but used for different contexts, like how many
  chats or cloud services allow you to be authenticated with multiple profiles at once. This
  implies to me that there may need to be a key trust scheme where the identity you use to log
  in somehow owns or controls the particular profile identities used in various group data spaces.
  I’d also like to consider how profile or key recovery would work, perhaps it would be possible
  to delegate authority to your family group for somebody to allow you to recover an account backup?
- The dream scenario for this is to have an app marketplace or social ecosystem where people
  have highly visible control over what data they generate and who has access to it. Social
  photo apps have an application identity that must request explicit access (via capabilities)
  to data you own in your namespace (or subspace), such as an album of photos to share. Feeds
  like reddit could be constructed by simply defining a convention for writing posts as documents
  in a namespace, then synchronizing those elements among the appropriate pool of peers (governed
  by the identity system that I’d like to create, which would be a first-class cryptographic trust
  toolkit).
