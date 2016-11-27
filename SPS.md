Secure Polling System 
by 
Jake Sternberg, Lilia Kai, James Halliday, April Glaser, Lesley Bell ,Jake McGinty  and Zaki Manian


A modernized Chaumian anonymous Internet voting system


Goals of the system


- Anonymous ballots
- Sybill Resistance
- Cryptographic verification of votes
- Censorship resistance




What is a blind signature?


A cryptographic signatures is a value affixed to a message that permits the origin and contents of the message to be authenticated. Cryptographic signatures are used to secure internet connections, conduct Bitcoin transactions and authenticate software. A blind signature is a variation of cryptographic signatures where the signer of the message produces a valid signature of a message without being aware of its contents. An effective blinding scheme prevents the signer of determining what message contents were even after being presented with the signed message.


Blind signatures are an important component of many anonymity schemes. In the original 1982 paper, David Chaum's proposed blind signatures primarily as a payment scheme[1] but he also proposed a voting system not entirely dissimilar to what is proposed here. While many of David cryptographic ideas for paper ballots have actually been implemented at scale. His electronic voting ideas have not been.


Goals of the project


The goals of this white paper is to document the design of a Chaumian voting system based on available cryptographic components and intended for the modern internet. These ideas can also be expanded to drive the state of Turing Complete consensus systems like Ethereum.


The Secure Polling System's goal is to permit casual yet robust expressions of public opinion and democratic decision making of small traditional organizations and novel cryptographic organizational forms like DAOs,DAPPs , etc. This system is neither intended nor suitable for high consequence decision making like national elections.


Flaws in verifiable voting.


Verifiable voting contains one very significant flaw. Unlike traditional ballots, the voter has a opportunity to selectively de-anonymize themselves and thus prove to third parties how they voted. This facilitates vote selling to motivated parties. On issues of consequence and little altruism among participants, verifiable voting becomes merely an elaborate scheme for gauging the economic interests involved in the outcomes.There are cryptographic schemes available to mitigate this like group signatures but they leave the registrars in a position to de-anonymize ballots at will. The Secure Polling System does not want to provide this power to the registrars.




Design of the Secure Polling System


The below is a skeleton of primary components of the Secure Polling System.


For more details see [2] & [3]


Components


Voter
The voters are individuals humans who wish to anonymously express opinions. Voters generate an anonymous cryptographic key pair that they will use to sign screeds.


Registrar
Registrars are charged with prevent voters from participating in a poll more than once and unknown users. The registrar executes a process by which an identified voter is able submit anonymous ballots to the system. If the registrar approves a voter to participate in the poll, the registrar executes a blind signature on the blinded public key. The registrar returns the signed public key to the voters. Because of the blinding process, the registrar will not able to identify the signed key.


There are not any implementations available in the open source community for David Chaum's original blind RSA proposal. Oleg Andreev has provided an implementation of blinded ECDSA for the Bitcoin curve.[4]




Tally


A tally server computes the total votes during the voting period. The tally server verifies that for each vote there is a valid key signed by a trusted registrar. The principle challenge faced in the design and implementation of the tally server is preventing screeds from being hidden. The canonical tally server implementation will accept a screed at any url and can be notified of urls containing screeds via any number of mechanisms.




Screed


A screed consists of a vote, a signature from a voter, a public key of the voter and a signature of a public key by the registrar.


Applications to blockchain consensus


There are many reasons why programs running embedded in a consensus system might want to use majority votes. In this formulation, the tally server runs as a smart contract, screeds are transactions to that smart contract and the registrars keys are pinned into to the smart contract during creation.


This system would be able to settle payments and perform other native functions of the consensus directly based on the outcome. Blockchain based voting systems face another challenge that the maintainers of the consensus(miners or other) have the option of censoring screeds by screening out transactions. This is a somewhat inherent downside. This mechanism could be limited by requiring a quorum of registered voters before the smart contract state updates.


Further cryptographic innovation offers some solutions in the form of non-interactive zero knowledge proofs. Voters could encrypt their screeds and submit them to the blockchain. They could share the decryption key for the screeds via on off blockchain mechanism. Any entity could then create a zero knowledge proof of the tally and submit this for verification by the smart contract.










[1]“Blind Signatures for untraceable payments” (Advances in Cryptology—Crypto ’82),
[2] https://github.com/securepollingsystem/securepollingoutline/blob/master/securepollingoutline.md
[3] https://www.newschallenge.org/challenge/elections/entries/secure-polling-system-is-a-real-time-accurate-and-transparent-system-for-empowering-citizen-driven-anonymous-participatory-polling-with-an-engaging-and-rewarding-experience-for-voters#
[4]
