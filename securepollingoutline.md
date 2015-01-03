###Secure Polling System overview and TODO list

1.  Glossary (as used to discuss the Secure Polling System)

  1.  Secure Polling System = the system described here, which allows for
      unmediated polling of unlimited issues, using strong cryptography to
      preclude fraud and guarantee anonymity to a group of people eligible
      to vote in a specific district 
  2.  district = a city, county, state, neighborhood, organization or
      group with an office which is enabling the Secure Polling System by
      performing the duties outlined below registrar
  3.  voter = a human individual who is eligible to vote in one or
      more districts
  4.  opinion = an exact text statement of up to 140 characters,
      expressing a belief, desire or opinion in the context of a voters'
      authority as a member of a democracy, for example:

    1.  “End all sales tax”
    2.  “End all sales tax!!!”
    3.  “Double all sales tax”
    4.  “Make the public transportation run all night”
    5.  “The mayor should step down immediately and a new election
    scheduled”

  5.  screed = a list of opinions a voter has chosen to express, with
    each opinion on its own line
  6.  registrar = an official (or office) who is trusted or oathed to do
    the following:

    1.  maintain 12 key pairs (a new one each month, expiring 12 months
        later)
    2.  determine eligibility of persons claiming to be voters (and checking
    their ID)
    3.  “sign” a binary blob (a blinded public key) for eligible persons
         with latest key
    4.  record the date and identity of persons' binary blob being signed
    5.  prevent persons getting signed again before their previous signature
        expires
    6.  publish public keys for all key pairs at all times

  7.  signed screed = a voter's screed, signed using their private key,
      and posted publicly along with their public key (for verification of
      their screed) and the registrar's (likely detached) signature of
      their public key
  8.  tally spider = a computer program which crawls the internet
      gathering signed screeds, verifying their signatures, and adding up
      the prevalence of opinions found in them
  9.  tally list = a list of opinions found by a tally spider, sorted in
      order of popularity, and presented with a percentage indicating how
      many signed screeds included it
  10. screed editor = a computer program used by voters to edit their
      screed, and to sign it and upload it to their web page so it can be
      found by tally spiders
  11. signature of public key = a detached signature, made by the
      registrar, of the public key of a voter. (this involves RSA
      blinding, so the registrar never sees the voter's public key)

2.  What it does

  1.  gives each voter total control over what they say and when they say
      it
  2.  allows voters to change their opinions and endorsements as often as
      they like
  3.  annual registration is quick and easy and can happen any convenient
      day
  4.  5.   

3.  How it works

  1.  software the voter uses

4.  What the voter does

  1.  voter learns about system and downloads an app, which contains all
      tools and a nice UI.  (some users will go to a website to do it and
      run the app in their browser)
  2.  voter creates a user in the app, which automatically generates a key
  3.  app tells user to identify registrar by supplying registrar’s
      signing key fingerprint
  4.  app downloads the registrar’s keys, and presents the full
      fingerprint of all the keys matching that key-id (hopefully just the
      one) so the user can verify the whole fingerprint if they care.
      This is only needed for blinding anyway so whatever.
  5.  app generates a random number for the blinding process, uses it and
      the registrar’s pubkey
  6.  process the voter goes through to generate their keypair, blind
      their pubkey, get it signed by registrar (appointment by email
      system), unblind the signature, associate it and their keypair with
      screed editor, run their tally spider (or have it pull a tally
      list from a trusted / cloud-based tally spider), open the tally list
      with tally list viewer and use the sorted results (or the screed of
      a friend with similar politics) to start their own screed.  Finally,

5.  Components list

  1.  GNUK = open-hardware USB device, which (generates and) holds the
      present (and only) private key used by the registrar.

    1.  Contains private key and NEVER exports it (it actually can't)
    2.  does all signing operations internally
    3.  modified to have an odometer-like counter, an LED, and a physical
        button
    4.  registrar has to physically press button at time of signing
    5.  signing causes numeric counter to increment, preventing fraud

  2.  keymanager = key management software for the registrar's office

    1.  talks to GNUK device when signing blobs for voters
    2.  keeps track of key pairs and posts public keys to official website
    3.  makes crypo usable by non computer-scientists for these tasks

  3.  screed editor / tally spider = actually one program.  For a voter to
      edit their screed they want to be able to start with a list of
      screeds already in-use by other people.  Since exact wording and
      spelling improve the system, voters benefit from copying existing
      opinions which are like memes.
  4.  tally list viewer = a program for viewing a tally spider list.
      It allows the user to subjectively group functionally identical
      opinions for purposes of interpreting opinion data.  In practice
      this means a person who feels that “end sales tax” and “abolish
      sales tax” are equivalent for their purposes can tell their computer
      to count these “different” opinions in the same column.  An
      important detail is that voters who express multiple opinions which
      are deemed identical will only be tallied as a single vote, in the
      spirit of one person one vote.  This applies even if the multiple
      opinions exist within seperate screeds on seperate websites.  The
      tally list viewer does this by merging screeds found with identical
      pubkey fingerprints.  For each fingerprint, each opinion can only be
      counted once, even if that voter lists it multiple times in one or
      multiple posted screeds.
  5.  tally spider list = this is a file, created by a tally spider,
      containing all the signed screeds found by a tally spider in a
      specific search.  each screed record includes the time it was
      accessed, an indication of whether both its signatures was verified,
      and an MD5 or SHA1 checksum of the record (so that pages which have
      not changed don’t have to be verified again if the MD5 matches).
      The record also includes the fingerprint of the voter’s public key,
      since this is how we watch for uniqueness of screed authors and
      merge screeds appearing with the same pubkey.

