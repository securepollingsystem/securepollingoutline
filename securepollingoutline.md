###Secure Polling System overview and TODO list

<ol><li>Glossary (as used to discuss the Secure Polling System)

<ol><li>Secure Polling System = the system described here, which allows for
      unmediated polling of unlimited issues, using strong cryptography to
      preclude fraud and guarantee anonymity to a group of people eligible
      to vote in a specific district 
</li><li>district = a city, county, state, neighborhood, organization or
      group with an office which is enabling the Secure Polling System by
      performing the duties outlined below registrar
</li><li>voter = a human individual who is eligible to vote in one or
      more districts
</li><li>opinion = an exact text statement of up to 140 characters,
      expressing a belief, desire or opinion in the context of a voters'
      authority as a member of a democracy, for example:

<ol><li>“End all sales tax”
</li><li>“End all sales tax!!!”
</li><li>“Double all sales tax”
</li><li>“Make the public transportation run all night”
</li><li>“The mayor should step down immediately and a new election
    scheduled”
</li></ol>

</li><li>screed = a list of opinions a voter has chosen to express, with
    each opinion on its own line
</li><li>registrar = an official (or office) who is trusted or oathed to do
    the following:

<ol><li>maintain 12 key pairs (a new one each month, expiring 12 months
        later)
</li><li>determine eligibility of persons claiming to be voters (and checking
    their ID)
</li><li>“sign” a binary blob (a blinded public key) for eligible persons
         with latest key
</li><li>record the date and identity of persons' binary blob being signed
</li><li>prevent persons getting signed again before their previous signature
        expires
</li><li>publish public keys for all key pairs at all times
</li></ol>

</li><li>signed screed = a voter's screed, signed using their private key,
      and posted publicly along with their public key (for verification of
      their screed) and the registrar's (likely detached) signature of
      their public key
</li><li>tally spider = a computer program which crawls the internet
      gathering signed screeds, verifying their signatures, and adding up
      the prevalence of opinions found in them
</li><li>tally list = a list of opinions found by a tally spider, sorted in
      order of popularity, and presented with a percentage indicating how
      many signed screeds included it
</li><li>screed editor = a computer program used by voters to edit their
      screed, and to sign it and upload it to their web page so it can be
      found by tally spiders
</li><li>signature of public key = a detached signature, made by the
      registrar, of the public key of a voter. (this involves RSA
      blinding, so the registrar never sees the voter's public key)
</li></ol>
<li><ol>What it does

<li>gives each voter total control over what they say and when they say
      it
</li><li>allows voters to change their opinions and endorsements as often as
      they like
</li><li>annual registration is quick and easy and can happen any convenient
      day
</li><li>anyone can count the number of people agreeing with each opinion,
      at any time and with independant verification</ol>

</li><li>How it works

<ol><li>using cryptographic signatures, lists of opinions from each voter
      can be proven traceable to a unique voter vetted by the registrar.
</li><li>anyone can use a tally spider (written in Golang) to curl all the
      signed screeds they can find on the net, and collate them by pubkey
</li><li>tally spider lists produced by spiders are shared publicly for 
      people who don't want to run a tally spider for themselves.
</li><li>voters use a program (in javascript, immobilized by hyperboot to
      prevent changes) to view opinions from a tally list and compare them
      with their own screed.  They can copy opinions from the tally list
      into their own screed (or compose new ones) and sign,save,post their
      updated signed screed.
</li><li>an advanced client for analyzing tally lists lets a person group
      functionally equivalent opinions for counting how many unique voters
      express an arbitrary position.  This is geared toward journalists and
      politicians who need to show and know the real meaning of counts.
</ol>

</li><li>What the voter does

<ol><li>voter learns about system and downloads an app, which contains all
      tools and a nice UI.  (some users will go to a website to do it and
      run the app in their browser)
</li><li>voter creates a user in the app, which automatically generates a key
</li><li>app tells user to identify registrar by supplying registrar’s
      signing key fingerprint
</li><li>app downloads the registrar’s keys, and presents the full
      fingerprint of all the keys matching that key-id (hopefully just the
      one) so the user can verify the whole fingerprint if they care.
      This is only needed for blinding anyway so whatever.
</li><li>app generates a random number for the blinding process, uses it and
      the registrar’s pubkey
</li><li>process the voter goes through to generate their keypair, blind
      their pubkey, get it signed by registrar (appointment by email
      system), unblind the signature, associate it and their keypair with
      screed editor, run their tally spider (or have it pull a tally
      list from a trusted / cloud-based tally spider), open the tally list
      with tally list viewer and use the sorted results (or the screed of
      a friend with similar politics) to start their own screed.  Finally,
</li></ol>

</li><li>Components list

<ol><li>GNUK = open-hardware USB device, which (generates and) holds the
      present (and only) private key used by the registrar.

<ol><li>Contains private key and NEVER exports it (it actually can't)
</li><li>does all signing operations internally
</li><li>modified to have an odometer-like counter, an LED, and a physical
        button
</li><li>registrar has to physically press button at time of signing
</li><li>signing causes numeric counter to increment, preventing fraud
</li></ol>

</li><li>keymanager = key management software for the registrar's office

<ol><li>talks to GNUK device when signing blobs for voters
</li><li>keeps track of key pairs and posts public keys to official website
</li><li>makes crypo usable by non computer-scientists for these tasks
</li></ol>

</li><li>tally spider = a program which runs on a server or EC2 instance
      and crawls all the URLs it can find which contain signed screeds.
      It collates the opinions it finds by the public key signing them.
      Its output is called a tally spider list and is web-accessible,
      so that people who need the data but don't want to run their own
      tally spider can get a predigested tally from someone they trust.
</li><li>tally spider list = this is a file, created by a tally spider,
      containing all the signed screeds found by a tally spider in a
      specific search.  each screed record includes the time it was
      accessed, an indication of whether both its signatures was verified,
      and an MD5 or SHA1 checksum of the record (so that pages which have
      not changed don’t have to be verified again if the MD5 matches).
      The record also includes the fingerprint of the voter’s public key,
      since this is how we watch for uniqueness of screed authors and
      merge screeds appearing with the same pubkey.
</li><li>tally list viewer = a program for viewing a tally spider list.
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
</li><li>screed editor = For a voter to edit their screed they want to
      be able to start with a list of opinions already in-use by other
      people.  Since exact wording and spelling are very important,
      voters benefit from copying existing opinions which are like memes.
      The screed editor lets the voter compare their screed with opinions
      found in a tally list, copying popular opinions that they agree with.
      Of course voters can also type in any opinion they want, which is
      how all popular opinions begin.
</li></ol>

</li></ol>
