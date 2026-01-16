Locking In
# Intro to HW Security
- HW Threats
	- IP Vendor --> System Integrator --> Manufacturer
	- IP Vendor --> Manufacturer
	- ![[Pasted image 20251210215817.png]]
	- Issues with third party IP design
		- These companies are located across the world, there is NO CONTROL on the design process. 
		- W
	- Old Design Process
		- ![[Pasted image 20251210215925.png]]
	- New Design Process
		- ![[Pasted image 20251210215933.png]]
		- Filled with untrusted IPs that could harm final product. 
	- Untrusted System Integrator
		- ![[Pasted image 20251210220048.png]]
	- Counterfeiting through binary and other methods. Over-produced IPs or Defective/Out-of-spec ICs can be used to counterfeit IP. 
	- IC Counterfeiting
		- Most prevalent attack today
		- Unauthorized production of wafers
		- It is estimated that counterfeiting is costing semiconductor industry more than billions of dollars a year
	- IC Recycling - PCBs can be taken off recycled ICs to be refined and resold 
	- Supply Chain Vulnerabilities
		- Design - Can be cloned, IP Piracy
		- Fabrication - Remarked Overproduction, Out-of-spec/Defective
		- Assembly - Out-of-spec/Defective
		- Distribution - Recycled, Remarked, Overproduction, Out-of-spec/Defective
		- Lifetime - Recycled, Remarked, Defective
		- End of life/ Recycling - Recycled, Remarked, Defective/Out-of-spec
# Intro to Cryptography
## Big Picture
- Cryptography protects confidentiality, integrity, authenticity, and non-repudiation of messages in hostile environments
- Core building blocks: substitution ciphers, transposition (permutation) ciphers, product ciphers, stream vs block cipher, symmetric vs asymmetric (public-key), DES, and AES
## Basic Terminology
- Plaintext $P$ is the original message, ciphertext $C$ is the encrypted message; encryption $E$ maps $P \rightarrow C$, decryption $D$ maps $C \rightarrow P$ with $P = D(E(P))$ 
- A key-based system uses $C = E(K_E, P)$ where $P = D(K_D, C)$; keys select specific algorithms from families of possible encryptors/decryptors
- Desired protocol properties: delivery, confidentiality, authenticity (prove who sent it), integrity (uncorrupted), uniqueness, and non=repudiation (sender cannot deny sending).
- ![[Pasted image 20251210231747.png]]
- ![[Pasted image 20251210231759.png]]
- 
## Threats and Cryptanalysis
- Message threats: interception, interruption (blocking), modification, and fabrication; defenses control underlying vulnerabilities. 
- Cryptanalysis goals; recover a message, deduce the key, find weakness in the algorithm or its implementation, and exploit statistical/language properties
- Common attack models: cipher-text only, known-plaintext, probable-plaintext, chosen-plaintext, and chosen-ciphertext attacks. 
	- Ciphertext-only attack: The attacker only sees one or more ciphertexts and tries to infer the plaintext or key just from those encrypted messages
	- Known-plaintext attack: The attacker knows some pairs of plaintext and corresponding ciphertext and uses those examples to infer the key or decrypt other ciphertexts. 
	- Probable-plaintext attack: The attacker can guess some likely structure or fragments of the plaintext (like standard headers) and uses those guesses plus the ciphertext to aid cryptanalysis
	- Chosen-plaintext attack: The attacker can choose arbitrary plaintexts and obtain their ciphertexts, then use these oracle queries to deduce the key or decrypt other messages
	- Chosen-ciphertext attack: The attacker can submit chosen ciphertexts to a decryption oracle (except possibly the specific target ciphertext) and uses the returned plaintexts to learn the key or recover other plaintexts. 
- Breakable encryption is theoretically possible to create unbreakable systems, but practically they're always breakable. 
## Classical Ciphers
- Substitution ciphers replace letters of P with other letters. Caesar's cipher uses a fixed shift, is monoalphabetic, and is easily broken by exhaustive search and frequency analysis. 
	- ![[Pasted image 20251210232126.png]]
- Statistical attacks use English letter frequencies (unigrams) and correlation functions to rank candidate keys. Caesar's short key makes frequencies visible
	- ![[Pasted image 20251210232141.png]]
	- ![[Pasted image 20251210232206.png]]
	- ![[Pasted image 20251210232211.png]]![[Pasted image 20251210232225.png]]
	- ![[Pasted image 20251210232219.png]]
- Polyalphabetic substitution (Vigenère) uses an n-character key, cycling through different substitution alphabets to flatten frequency distributions and resist simple frequency attacks. 
	- ![[Pasted image 20251210232238.png]]
- Vigenère tableaux: rows are shifted alphabets; each plaintext charavter and key character pick a table cell (row from P, column from key) to produce C:
	- https://www.youtube.com/watch?v=SkJcmCaHqS0
## Transposition and product ciphers
- Transposition ciphers permute the positions of letters without changing them (e.g. columnar transposition, rain-fence); the key is often the number of columns/rails
	- ![[Pasted image 20251210232303.png]]
	- ![[Pasted image 20251210232317.png]]
	- 
- Product ciphers compose multiple substitution/transposition stages $E_n(...E_2(E_1(P)))$; they aim to combine confusion and diffusion but are not automatically stronger than components. 
## Good cipher properties and requirements
- Substitution provides confusion (hides direct mapping from P to C), transposition provides diffusion (scrambles n-grams), and products provide both if well-designed.
- Commercial design principles: sound mathematics, expert (including external) review, and survival over time under attack; DES, RSA, and AES are examples that followed such processes
- Practical systems must balance security with resource constraints (no supercomputers on the battlefield) and intended application. 
## Stream Ciphers
- Stream cipher: encrypts P symbol-by-symbol using a key stream (e.g. polyalphabetic with repeated key); has low latency and low error propagation but low diffusion and is vulnerable to cut-and-paste modification. 
	- ![[Pasted image 20251210232527.png]]
- Stream cipher fragility: key misalignment (dropped key character) corrupts all subsequent decrypted symbols until resynchronization is somehow recovered
	- 
- Block cipher: encrypts blocks of characters/bits at a time (e.g. columnar transposition, DES, AES) and offers high diffusion and easy detection of insertions/modifications but higher latency and error propagation within a block.
	- ![[Pasted image 20251210233135.png]]
- Some block constructions allow partial early decryption "weak block", others require the whole bock before any plaintext can be recovered  "strong-block". 
	- Weak block: the receiver can start recovering some plaintext bytes as soon as part of the corresponding ciphertext block arrives, so decryption can be done "on the fly"
	- Strong block: mapping from plaintext to ciphertext is coupled across the entire block, so receiver must have the complete block before any of the original plaintext can be correctly recovered, which increases latency but increases strength
## Symmetric vs Asymmetric Encryption
- Symmetric (secret key): same key used for encryption and decryption ($K_E = K_D$); fast and suitable for bulk data, but key distribution and storage scale as $O(n^2)$ for n users
- Asymmetric (public-key): different public and private keys ($K_E \ne K_D$); public keys can be openly distributed, private keys kept secret, reducing total keys to $2n$ for n users
- Public-key requirements: easy to encrypt/decrypt with the right key, but infeasible to compute the private key from the public key or from chosen-plaintexst attacks
- In PKE, key pairs satisy relation like $P = D(k_{priv}, E(k_{pub}, P))$ and often also the reverse $P = D(k_{pub}, E(k_{priv}, P))$, enabling both confidentiality and digital signatures. 
- Symmetric crypto is still needed because PKE operations (e.g. exponentiation) are orders of magnitude slower than bit-level symmetric operations (XOR, shifts, table lookups)
	- ![[Pasted image 20251210234220.png]]
## Data Encryption Standard (DES)
- DES is a 1970s NIST standard block cipher for 64-bit blocks with an effective 56-bit key; it is a product cipher with 16 rounds of substitution and permutation using round keys derived from the user key.
- Round structure: input block is split into 32-bit halves $L_i$, $R_i$; each round transforms $R_i$ with a subkey $K_i$ via S-boxes and permutations, then swaps halves so $R_i$ becomes $L_{i+1}$
- Key schedule uses permutations (PC-1, PC-2), left shifts, and selection of 48-bit round keys from the 56-bit key; remaining bits are sometimes used for parity/error checking
- DES's main weakness is its short key: $2^{56} \approx 10^{15}$ keys, feasible for large distributed or specialized hardware; actual attacks have demonstrated exhaustive search in only days
- Double DES (two encryptions with different keys) provides far less than 112 bits of security, because of the (meet-in-the-middle) shortcut attack
	- More concretely: with Double DES, you compute $C = E_{K_2}(E_{K_1}(P))$, so naïvely the key space is $2^{56} \times 2^{56} = 2^{112}$ possibilities and you might think an attacker needs on the order of $2^{112}$ work to brute‑force it. In reality, a meet‑in‑the‑middle attack uses a known $P, C$ pair, precomputes all possible “middle” values $M = E_{K_1}(P)$ for every $K_1$, and independently computes all possible $M' = D_{K_2}(C)$ for every $K_2$, then matches them; this costs about $2^{56} + 2^{56} \approx 2^{57}$ time and $2^{56}$ memory instead of $2^{112}$, so the effective security gain over single DES is small rather than exponential.
- Triple DES with an E-D-E pattern and two keys achieves an effective ~112 bit security level, requiring an order of $2^{112}$ combinations to brute force
	- ![[Pasted image 20251210234840.png]]
	- ![[Pasted image 20251210234848.png]]
## AES
- Because DES became vulnerable, NIST ran the AES content for an open, royalty-free, symmetric block cipher with 128-bit blocks and key sizes of 128/192/256 bits; Rijndael was selected after multi-round public evaluation.
- AES operates on 128-bit blocks through a series of rounds: SubBytes )nonlinear substitution), ShiftRows (row-wise transportation), MixColumns (linear mixing over finite field) and AddRoundKey (XOR with round key)
- Number of rounds depends on key size (~10 rounds for 128-bit keys); design emphasizes strong confusion/diffusion, efficient software and hardware implementations, and extensibility by increasing key length or rounds
## Motivation and structure of public-key crypto
- Diffie-Hellman introduced public key cryptosystems; each user has one private key and one corresponding public key, bringing key counts from $O(n^2)$ to $O(n)$ 
- In such systems anyone can encrypt with a recipients public key but only the holder of the matching private key can decrypt, solving the secure key-distribution problem for confidentiality
## RSA Basics
- RSA (Rivest-Shamir-Adleman, 1978) is the most prominent public-key scheme, based on the hardness of factoring large integers and modular exponentiation arithmetic.
- Public key: modules $n$ and exponent $e$ ; encryption $C = P^e\mod n$ , and decryption uses private exponent $d$ with $P = C^d \mod n$ where computing  $d$ without factoring $n$ is believed infeasible.
- RSA remains secure in practice with appropriate key sizes, though future large-scale quantum computers could threaten it via factoring algorithms 

# Basics of VLSI Design and Testing
## Technology scaling and nanoelectronics
- Feature sizes have shrunk from micrometers to below 100 nanometers, enabling silicon microelectronics, silicon nanoelectronics, and emerging non-silicon devices like CNTs and quantum devices
- As devices scale, power density keeps rising, process variation worsens, and designs must address leakage, dynamic power management, DFM, and reliability
	- ![[Pasted image 20251211000522.png]]
	- 
## IC Design and Test Flow
- Design Spec --> IC Design --> Fabrication --> Wafer test --> Assembly --> Burn in --> Package test --> Customer
- ![[Pasted image 20251211000610.png]]
## Nanoscale VLSI System
- VLSI System (e.g. Baseband processor, DSP, ASIC) --> Architectural blocks --> Interconnects --> Transistors (mosfet)
- System --> Architecture --> Logic --> Transistor
- ![[Pasted image 20251211000714.png]]
## VLSI Applications and Constraints
- Different applications trade off power and performance: ultralow power (medical, space, sensors), portable (mobile, wireless, multimedia), and high-performance general purpose computing (servers, real-time systems)
- Design constraints include budget, silicon area, dynamic/static power, speed, and schedule/time-to-market, often optimized using design automation (CAD, EDA)
## Transistors
- Modern ICs are built from nanoscale MOS transistors, nMOS turn "on" when gate is high, pMOS when gate is low, and combining them yields CMOS logic with no statis DC path and lower static power
	- ![[Pasted image 20251211001015.png]]: nMOS
	- ![[Pasted image 20251211001037.png]]: pMOS
	- ![[Pasted image 20251211001047.png]]: cMOS
- CMOS Inverters and logic gates (NAND, NOR, XOR) implement Boolean algebra, Demorgan's theorems and truth tables guide equivalent implementations and simplifications
## Technology Scaling and Performance
- Scaling device dimensions by ~30% doubles transistor density, reduces oxide thickness (faster devices) and lowering $V_{DD}/V_i$ reduces active power, but variability and interconnect delay dominate at advanced nodes
- Interconnect (wires) now significantly contributes to delay, so wire lengths and aspect ratios are critical to timing closure
## From IC Concept to Silicon
- VLSI design creates an IC from system specs through function/behavioral description, then logic design, to circuit design, to physical design, to fabrication, then packaging, and testing
	- ![[Pasted image 20251211001409.png]]
	- ![[Pasted image 20251211001415.png]]
- Design automation tools (synthesis, PNR, verification) are needed because of huge component counts and to optimize speed, power, and area under tight schedules
## Semiconductor and Wafer Processing
- Transistors and interconnect are formed via semiconductor processing steps: oxidation, diffusion/implantation, and deposition controlled by photolithography masks
	- https://www.youtube.com/watch?v=LWfCqpJzJYM
- Wafers go through inline tests, wafter test (with potentially large early fall-out), assembly/packaging, then burn-in at elevated temperatures to weed out early-life failures
## Design Styles and Methodology
- Design methodologies: 
	- Full custom: max performance, highest cost
	- Standard cell: Faster, less performance
	- Gate Array
	- FPGA
- Design process is iterative: specification --> behavioral/RTL --> synthesis --> PNR --> verification (simulation/emulation at behavioral and structural levels) --> fabrication --> test
## Yield Defects, and Why Testing Matters
- Yield $Y$ is the fraction of manufactured dies that are good. defects create bad chips, so yield depends on defect density and distribution (clustered vs unclustered)
	- ![[Pasted image 20251211002116.png]]
- Testing separates bad from good chips; inadequate test increases field failures, while over-aggressive test can cause yield loss by rejecting good parts, so test costs and ROI must be balanced
	- ![[Pasted image 20251211002205.png]]
## Testing Basics and Levels
- Basic digital testing principle: apply input patterns to a circuit-under-test (CUT), compare outputs with stored correct responses using a comparator, and flag mismatches
	- ![[Pasted image 20251211003255.png]]
- Testing occurs at multiple levels (chip, board, system, system-in-field); costs typically increase about  10x as you move up each level, motivating early, chip-level test and DFT (Design For Testability)
## Ideal vs Real Tests
- Ideal tests would detect all manufacturing defects and never reject good parts, but real tests rely on analyzable fault models and cannot cover all real defects. 
- Imperfect testing leads to high yield loss and high defect level, both of which must be managed
- Scan Flop Flops:
	- ![[Pasted image 20251211003632.png]]
	- ![[Pasted image 20251211003639.png]]
	- ![[Pasted image 20251211003643.png]]
	- 
## Digital Fault Models
- A fault model describes how defects manifest at a logical or physical level; common models include single stuck-at-faults, transistor stuck-open/short, bridging faults, delay faults, and memory-specific coupling/pattern-sensitivity faults
	- ![[Pasted image 20251211003717.png]]
	- ![[Pasted image 20251211003721.png]]
- A given fault model has $k$ types of faults, and a given circuit has $n$ possible fault sites.
- Multiple Fault Model - circuit can have multiple faults, specifically $(k+1)^n-1$, and each fault site can have 1-of-$k$ fault types, or be fault-free. The $-1$ represents the fault-free circuit
- Single-fault models assume only one fault is present; they drastically reduce the number of combinations compared to multiple-fault models, yet often give good coverage. The number of single faults is $k \times n$
 - Equivalent faults have one or more single faults that have identical behavior for all possible input patters, so only one fault from a set of equivalent faults needs to be simulated
 - Fault collapsing removes equivalent faults except for the one to be simulated, reducing the total number of faults, fault simulation time, and test pattern generation time
## Stuck-at and Fault Collapsing
- Stuck-at-0 and stuck-at-1 model a line permanently tied to logic 0 or 1; for a circuit with $n$ possible sites at $k = 2$ fault types, single stuck-at faults are $2n$
- Equivalent faults behave identically for all inputs, so fault collapsing removes redundancies, often reducing stuck-at-fault counts by 50-60% and transistor fault counts by ~24-35%
	- ![[Pasted image 20251211004358.png]]
## Transistor, shorts, and opens
- Transistor faults: stuck-short (on) causes unintended current paths, detectable with IDDQ tests, and stuck-open (off) causes stored/"last value" behavior requiring two-vector test
- Wire opens map to stuck-open or stuck-at behavior depending on where they occur; shorts between wires are modeled as bridging faults (wired-AND/OR, dominant, dominant-AND/OR) can also be IDDQ-detectable
- ![[Pasted image 20251211004606.png]]
## Delay, Crosstalk, and Memory Faults
- Path-delay fault models capture excessive propagation delay along paths; two-vector tests induce transitions and detect timing violations. 
	- ![[Pasted image 20251211004800.png]]
- Crosstalk, capacitive/inductive coupling between interconnects. can introduce glitches/delays; memory tests target pattern-sensitivity and coupling faults using structured March algorithms and background data sequences. 
	- ![[Pasted image 20251211004830.png]]
	- ![[Pasted image 20251211004839.png]]
## Test Generation, Coverage, and ATGP
- A test is a sequence of test vectors whose outputs are monitored, exhaustive and full tests are infeasible for large designs.
- Fault coverage measures what fraction of modeled faults are detected; reject rate depends on yield and fault coverages, so efficient tests maximize coverage with minimal patters
- Automatic Test Pattern Generation (ATGP) algorithms generate vectors for a given CUT and fault model; fault simulation (parallel, deductive, concurrent) estimates coverage and guides test set to improvement
## Scan, Boundary Scan, and BIST
- Scan design converts flip-flops into scan flip-flops connected in chains in scan mode, primary inputs plus scan-in control internal states, and scan-out exposes internal responses, enabling highly efficient structural tests
	- ![[Pasted image 20251211005219.png]]
	- 
- Boundary scan applies to scan concepts to I/O cells for board-level interconnect test; IEEE TAP (TCK, TMS, TDI, TDO) provides a standard interface
	- ![[Pasted image 20251211005234.png]]
- Built-In Self-Testing embeds a test pattern generator and output response analyzer on-chip so the device can test itself at a chip, board, system, or field level, improving test access and reducing reliance on expensive external testing equipment. 
	- ![[Pasted image 20251211010109.png]]
## Design For Testability
- DFT improves controllability/observability of internal nodes using ad-hoc test points, structures scan, boundary scan, and BIST to make comprehensive yet affordable tests possible
- Incorporating DFT from the beginning is effectively a "contract" requirement between design house and fab: without a credible test strategy, manufacturing and product quality goals cannot be met
# PUFs and TRNGs
## Motivation & Threat Model
- Traditional designs store long-term secret keys in NVM, which are vulnerable to physical extraction, invasive probing, and require extra power/area and secure key-injection in manufacturing
- Attackers target crypto keys, algorithms, and sensitive data in RAM/ROM and may also modify stored values (balances), so hardware must provide authentication, confidentiality, and integrity even on constrained platforms like RFIDs
## From Mathematical OWFs to Physical OWFs
- Asymmetric cryptography and hash functions rely on mathematical one-way functions (easy forward, hard inverse) and one-way hashes (avalanche, preimage/collision resistance), but these assumptions face technological (parallelism, reverse engineering), fundamental (possible future algorithms, quantum factoring), and practical (embedded constraints) challenges.
- Physical One-Way Functions (POWFs) use complex, chaotic physical structures that are cheap to fabricate, hard to clone or model, and naturally tamper-resistant, avoiding permanent digital secrets. 
## Modules
- Hardware Security Model (HSM) is a physical computing device that safeguards and manages digital keys for strong authentication and provides crypto processing. These modules traditionally come in the form of a plug-in card or an external device that attaches directly to a computer or network server
	- ![[Pasted image 20251211014731.png]]
- Trusted Platform Model (TPM) is a specialized chip on an endpoint device that stores RSA encryption keys specific to the host system for hardware authentication. Each TPM chip contains an RSA key pair called the Endorsement Key (EK)
	- ![[Pasted image 20251211014724.png]]
## PUF Basics and Silicon PUFs
- A Physical Unclonable (Random) Function (PUF) is a physical function that is easy to evaluate, output-wise indistinguishable from random, and unpredictable even with physical access because it exploits manufacturing variability 
	- ![[Pasted image 20251211014752.png]]
- Silicon PUFs use delay differences from random process variation: nominally identical paths or circuits (e.g. on FPGAs) exhibity unique delay distributions, enabling chip identification without storing keys. 
	- ![[Pasted image 20251211015003.png]]
	- ![[Pasted image 20251211015011.png]]
- NO 2 ICs ARE THE SAME
## PUF Security & Attacks
- Modeling/software attacks aim to approximate the PUF mapping; experiments show that even for simple arbiter PUFs, best models still incur noticeable error, suggesting hard modeling in practice
- Physical attacks include invasive modifications that alter delays (destroying the PUF) and fine grained non-invasive timing analysis, but wire delay depends on challenge and neighbor activity, complicating precise extraction. 
## Ring Oscillator (RO) PUFs
- RO PUFs use array of nominally identically ring oscillators whose frequencies differ due to process and environmental variation comparing frequencies of selected pairs yields response bits
	- ![[Pasted image 20251211015957.png]]
- Stability improves by using only "stable" RO pairs whose base frequencies differ significantly; enrollment can characterize all pairs and select those with large margins for subsequent use
## PUF Metrics and Experiments
- Inter-chip variation: fraction of differing bits between two PUFs for the same challenge set; ideal uniqueness is about 50% if outputs are IID Bernoulli(0.5)
- Intra-chip (environmental) variation: fraction of bits that change when regenerating responses on the same chip under varying conditions; ideally near 0% for good reliability
	- ![[Pasted image 20251211020240.png]]
	- ![[Pasted image 20251211020246.png]]
- FPGA Experiments with 1024 ROs showed average inter-chip variation close to 50% and intra-chip variation well below 1% across temperature and voltage changes, supporting both uniqueness and stability. 
- ![[Pasted image 20251211020418.png]]
- ![[Pasted image 20251211020425.png]]
## PUF Application: Authentication and Keys
- For authentication, a trusted part stores many CRPs per device; later, it issues a fresh challenge and checks if the response matches the database (within some error margin), using each challenge at most once to prevent replay and modeling
	- ![[Pasted image 20251211022656.png]]
- Directly using raw PUF outputs as cryptographic keys is problematic due to noise and lack of structure; instead, key generation uses an initialization step (capture PUF output and store ECC syndrome) and regeneration step (correct new PUF output using public syndrome), then derive keys via a one-way function. 
	- ![[Pasted image 20251211022608.png]]
	- 
## Controlled PUFs (CPUFs) and CRP Management
- A controlled PUF wraps a PUF with control logic so responses are never revealed directly; instead, the interface exposes functions like GetSecret and GetResponse that combine responses with program hashes
	- ![[Pasted image 20251211022904.png]]
- Bootstrapping: the manufacturer queries CPUF with pre-challenge and program, obtains hidden response and computes the public challenge as a has of program and pre-challenge, then stores (challenges, response) while discarding the pre-challenge. 
	- ![[Pasted image 20251211022910.png]]
## CPUF Applications
- Software licensing: encrypted code (Ecode) is bound to a specific chip by encrypting with a secret derived from CPUF (Secret = Hash(Hash(Program), Response)) and only that chip can reconstruct the secret and run the code
- CPUFs provide volatile shared secrets that are hard to model as long as physical attacks cannot reveal raw PUF responses; the PUF obscures control logic, and the control layer prevents direct CRP leakage
## TRNGs and Randomness in Crypto
- True Number Random Generator (TRNG)
	- ![[Pasted image 20251211023655.png]]
- Many cryptographic primitives require high-quality randomness: one-time pads, symmetric keys (AES,DES), RSA primes, session keys, private keys, and initialization vectors used in ciphers
- Pseudo-RNGs expand a short true random seed algorithmically; they are deterministic and seed-dependent, so a secure system still needs a true entropy source
## Physical Entropy and TRNG Design
- true randomness arises from physical phenomena such as radioactive decay, thermal/shot noise, and other microscopic processes; algorithms alone cannot produce true randomness
	- ![[Pasted image 20251211024200.png]]
- A good TRNG architecture consists of an entropy source, a simple harvesting mechanism that minimally perturbs the source, and optional post-processing to remove bias and improve robustness
- Set of Requirements:
	- Design should be purely digital
	- Harvesting mechanism should be simple - the unpredictability of the TRNG should not be based on the complexity of the harvesting mechanism but the unpredictability of the entropy source
	- No correction circuits
	- Compact and efficient design (high throughput per area and energy spent)
	- The design should be sufficiently simple to allow rigorous analysis
- Example: Clock Jitter
	- ![[Pasted image 20251211024426.png]]
## RO-based TRNG Architecture
- A typical design uses many ROs whose phase jitter is influenced by process and environmental variations; an LSFR may inject switching noise to amplify the environmental randomness
	- ![[Pasted image 20251211024559.png]]
	- ![[Pasted image 20251211024612.png]]
	- 
- A selection/operation unit (XOR of sampled RO outputs) translates random phase into bits, and a capture unit samples at a suitable rate to produce true random sequence
## Keyless Secure Sensing via PUFs
- PUF outputs are temperature-sensitive, so a strong PUF can serve as a "keyless" temperature sensor: Bob pre-measures CRPs at a discrete temperature and later verifies an object's temperature by comparing live responses to stored lists
- This yields "virtual proofs" of temperature (and more generally of physical conditions) without any stored secret key, re-using PUF properties (unclonability, many CRPs, non-modelability) in a sensing context
# Hardware Metering
## Motivation
- Fabrication defects reduce yield; foundries predict yield and discard failing chip after test, but as feature sizes shrink, defects and their clustering become more problematic
- Every stage (IP vendor, foundry, system integrator, assembly) can be untrusted, enabling IP piracy, overproduction, cloned/remarked/recycled parts , defective or out-of-spec chips entering the market
## Need for Hardware Metering
- Traditional production flows give the foundry full design, test patterns, and expected responses with little visibility for the IP owner into actual yield or number of produces/activated chips
- Hardware metering defines protocols that let IP owners uniquely tag and/or control ICs post-fabrication
- ![[Pasted image 20251211030831.png]]
## Passive Metering
- Passive metering tags chips but does not give the designer control; the IP owner later samples parts from the field and checks identifiers against a database
	- ![[Pasted image 20251211031014.png]]
- **Nonfunctional identifiers** store IDs separate from functionality; reproducible tags (stamped or stored serial numbers) are easy to track but trivial to clone, while unclonable identifiers use process variations (ICID - threshold mismatches in array of transistors incurred different currents and therefore unique random numbers, PUF-based, or intrinsic leakage/power/timing signatures) that are unique but still allow overproduction and rely on honest reporting
	- ![[Pasted image 20251211031317.png]]
## Functional Metering
- Functional metering ties identifiers to internal functional behavior (FSM states/paths) so each chip has a unique "functional signature" while I/O behavior remains identical
- Practical schemes fabricate from a single mask but keep one programmable path; extra states or redundant states are added and post-silicon programming selects per-chip internal behavior; this is area-expensive, hard to test, and typically requires re-activation at a trusted site. 
## Active Metering: General Idea
- Active metering lets the IP owner enable, control, or disable ICs by requiring a secret key or sequence  for correct operation; inactivated chips are non-functional
- Two types, Internal and External
	- Internal active metering embeds locks into FSM/state space (no external crypto)
	- External active metering uses cryptographic blocks (RSA, PUF/TRNG) and key exchange to lock functionality
## Internal Active Metering and State Obfuscation
- State-space obfuscation hides states/transitions so normal behavior is reached only under a secret key, transforming both FSM and combinational logic and adding monitors to detect incorrect operations. 
	- ![[Pasted image 20251211031616.png]]
	- 
- Adding a few flip-flops exponentially increases the number of FSM states, enabling many "locked" or "dummy" states; IP owner-known sequences (or PUF-initialized random states) are requires to steer the FSM back to functional states (Boosted FSM, Remote Activation)
	- ![[Pasted image 20251211031732.png]]
	- ![[Pasted image 20251211031804.png]]
	- ![[Pasted image 20251211031923.png]]
- These FSM-based methods are strong against cloning but weak against over-production and defective/out-of-spec ICs because chips must be activated before testing and the same key can often unlock many devices
## External Active Metering: EPIC
- EPIC (Ending Piracy of Integrated Circuits) inserts XOR gates on selected non-critical paths and drives them with a common key (CK): with the correct key, the circuit behaves normally, otherwise outputs are wrong.
	- ![[Pasted image 20251211032108.png]]
- Each chip includes RSA and a TRNG that generates a per-chip Random Chip Key (RCK); the IP owner uses RCK-public and a master private key to encrypt CK into an input key (IK), which each chip decrypts internally to recover CK
	- ![[Pasted image 20251211032116.png]]
- EPIC is effective against cloned ICs (adversaries lack CK) but fails against over-produced, out-of-spec, and defective ICs because the foundry can misreport yield to obtain extra IKs and all chips are activated before testing
## External Active Metering: Logic Barriers (LB)
- Logic Barriers partition the design so all paths from inputs to outputs go through reconfigurable logic (LUTs) that implement a "barrier"; only the correct key configures the LUTs to realize the intended function
	- ![[Pasted image 20251211034055.png]]
- IP owner sends the fixed portion ($F_{fixed}$ plus barrier locations) to foundry and keeps $F_{reconfig}$ (barriers configuration + key); each chip uses a PUF/TRNG to generate a public/private key pair so that the same common key is encrypted differently per chip
	- $F_{fixed}$ is the physical circuit with reconfigurable logic (logic barriers) inserted and it is sent to the foundry to be fabricated.
	- $F_{reconfig}$ refers to the locations of the inserted reconfigurable logic which only the IP owner knows
	- ![[Pasted image 20251211034353.png]]
- LB has similar strengths/weaknesses as EPIC (good against cloning, poor against over-production/defects) and incurs significant area/timing overhead because LUT barriers can be about 5$\times$ larger than XOR-based schemes
## Secure Split-Test (SST)
- SST modifies the test flow so the IP owner participates in testing and activation; three-input XOR gates (XOR masks) are inserted before flip-flops on no-critical paths, controlled by internal signals IN1 and IN2
	- ![[Pasted image 20251211034556.png]]
	- ![[Pasted image 20251211034602.png]]
- During test, patterns and masked responses are exchanged between foundry and IP owner; only after the IP owner verifies which chips are good does it provide final keys FKEYs to make selected chips permanently functional
- SST is effective against over-produces, cloned, defective, and (with added sensors) out-of-spec ICs, because the IP owner knows exact pass/fail counts and only activates a controlled number of fully compliant chips
## FSM-based Remote Activation w/ PUFs
- In Boosted FSM, additional "trap" states are inserted so powering on sends the machine into one of many added states; only a secret input sequence (known to the IP owner) brings it to functional states.
	- ![[Pasted image 20251211034752.png]]
- Remote Activation reduces overhead by duplicating existing functional states instead of adding many dummy states; a PUF or Random Unique Block (RUB) initializes the FSM, and the IP owner computes a key that, when XORed with RUB, transitions the FSM onto the correct path
	- ![[Pasted image 20251211035753.png]]
	- ![[Pasted image 20251211035806.png]]
- Both schemes rely on PUF stability, and still do not address over-production or defective/out-of-spec ICs, since activation generally happens before complete testing
- Passive metering is simple but offers little real control; active metering with cryptography or FSM locking can thwart cloning but often overlooks test economics and yield manipulation.
- Secure Split-Test explicitly integrates test into the security protocol, giving the IP owner visibility into yield and final activation, and is therefore one of the few schemes that simultaneously addresses cloning, over‑production, defective, and out‑of‑spec parts.
# Hardware Trojans
## Threats
- Hardware Trojans are malicious additions or modifications to a circuit that can change functionality, reduce reliability, or leak sensitive information, and they can be inserted at IP vendor, design house, foundry, or deployment stages
	- ![[Pasted image 20251211124835.png]]
	- 
- Security-critical domains such as military, aerospace, finance, transportation, IoT, and commercial devices are prime targets because chips are often fabricated in untrusted, globalized supply chain
## IC/IP Trust Problem
- IP trust: untrusted third-party IP vendors may embed Trojans in soft/firm/hard IP cores; SoC integrators must verify that IP matches specifications and does nothing more despite lacking a golden implementation model/
	- ![[Pasted image 20251211125057.png]]
	- 
- IC/system trust: untrusted foundries or CAD tools may alter layouts or insert Trojans during fabrication; conventional manufacturing tests target random defects, not intentional extra logic, and struggle to detect tiny, dormant, or rare-event Trojans
	- ![[Pasted image 20251211125114.png]]
	- ![[Pasted image 20251211125234.png]]
	- 
## Example Trojans and Difficulty
- "Silicon Backdoor" example: an added RF antenna and interface leak secrets or receive commands without changing digital functionality, so standard functional tests cannot see it
	- ![[Pasted image 20251211125151.png]]
	- ![[Pasted image 20251211125242.png]]
- "Silicon Time Bomb" example: a counter/FSM or parametric weakening of wires/transistors quietly degrades or triggers catastrophic failure after specific conditions or aging, causing reliability issues rather than immediate functional errors
	- ![[Pasted image 20251211125442.png]]
## Trojan Taxonomy
- ![[Pasted image 20251211125922.png]]
- Physical characteristics:
	- Type: functional (added/removed gates) vs parametric (modified wires, transistor strengths, power grid) targeting reliability or performance
		- ![[Pasted image 20251211125930.png]]
	- Size: number/size of added or modified components; smaller Trojans are harder to detect but easier to activate
		- ![[Pasted image 20251211125939.png]]
	- Distribution: tightly clustered vs loosely scattered across layout, constrained by available dead space/filler cells/decaps
		- ![[Pasted image 20251211125951.png]]
	- Structure: layout-preserving Trojans vs those requiring layout regeneration and changing chip form factor, which affects power/delay and can aid detection
		- ![[Pasted image 20251211125958.png]]
- ![[Pasted image 20251211130011.png]]
- Activation Characteristics: externally activated (antenna/receiver, scan or register access) vs internally activated (always-on or condition-based via sensors, state, inputs, counters)
	- ![[Pasted image 20251211130021.png]]
- ![[Pasted image 20251211130030.png]]
- Action Characteristics: modify function (wrong outputs, bypass logic), modify specification (noise, delay, temperature, defects, reliability degredation), or transmit information (covert channels). 
	- ![[Pasted image 20251211130039.png]]
## IP Trust Techniques (Pre-Synthesis)
- Formal verification uses property checking, equivalence checking, and model checking to prove RTL/Netlist/GDSII conform to the spec; any failing assertions are marked as suspicious for deeper review
- Coverage Analysis (line, statement, FSM, toggle, and assertion coverage) identifies unexercised code or states; uncovered portions and failed assertions are marked as suspicious for deeper review
	- ![[Pasted image 20251211130546.png]]
## IC Trust and Detection Approaches
- ![[Pasted image 20251211130713.png]]
- Destructive methods (reverse engineering with delayering and SEM) compare layout to a golden IC/Layout but are expensive, time-consuming, and sample only a few dies
- Non-destructive methods:
	- Run-time monitoring exploits redundancy and in-system checkers to detect abnormal behavior and isolate/bypass infected regions
	- Test-time authentication uses logic testing and side-channel analysis at manufacturing or incoming inspection
## Logic-Testing Based Detection
- Logic testing aims to generate test vectors that activate Trojans and propagate their payload effects to primary outputs; both structural and functional patterns can be used
- Limitations: rare-event triggers and low-controllability/low-observability nodes make activation hard, sequential/externally triggered Trojans are especially difficult, and exhaustive functional test is infeasible for large designs
- MERO statistically targets rare nodes: it identifies low-probability internal events and generates patterns to drive each such node to its rare value N times, increasing the chance of triggering unknown Trojans at the cost of longer test sequences
	- ![[Pasted image 20251211131040.png]]
## Side-Channel Based Detection
- Side-channel approaches observe Trojan-induced changes in physical parameters such as leakage current (IDDQ), transient/switching current (IDDT), path delay, and EM emissions
	- IDDQ: Extra gates will consume leakage power.
	- IDDT: Extra switching activities will consume more dynamic power.
- Power Analysis: partial activation of Trojan logic increases local current draw; techniques like current-difference at power pads, charge integration over time, and locality analysis can distinguish Trojan-free vs Trojan-inserted dies, but process variation and measurement noise complicate small-Trojan detection and require golden chips and calibration
	- ![[Pasted image 20251211132216.png]]
	- ![[Pasted image 20251211132345.png]]
	- ![[Pasted image 20251211132355.png]]
	- 
- Delay Analysis: extra gates/capacitance change path delays; methods include shadow-register architectures and clock-sweeping to find start-to-fail frequencies, and are attractive because full Trojan activation is not requires, through process variations and pattern generation remain challenges
	- ![[Pasted image 20251211132228.png]]
	- ![[Pasted image 20251211132405.png]]
	- ![[Pasted image 20251211132411.png]]
	- ![[Pasted image 20251211132418.png]]
	- ![[Pasted image 20251211132426.png]]
	- 
- Comparative table shows logic testing is robust under process noise and good for ultra-small Trojans, while side-channel analysis is better for large Trojans with simpler pattern generation; combining both plus online validation gives better coverage
	- ![[Pasted image 20251211132311.png]]
	- ![[Pasted image 20251211132316.png]]
	- ![[Pasted image 20251211132326.png]]
	- ![[Pasted image 20251211132443.png]]
## Design for Hardware Trust
- Because detection alone is hard, design-for-trust techniques modify designs to ease detection or prevent insertion:
	- Rare-event removal and dummy-scan/functional flip-flop insertion increase controllability/observability of low-transition nodes, improving full-action, power, and delay-based test coverage
		- ![[Pasted image 20251211133020.png]]
	- Design obfuscation (state-space obfuscation) adds an obfuscated mode and key-dependent transitions so only a secret sequence brings the FSM into normal operation, complication reverse engineering and making many Trojans ineffective
		- ![[Pasted image 20251211133029.png]]
	- BISA (Built In Self-Authentication) fills all unused layout spaces with self-testable logic, leaving no free area to stealthily insert Trojans
		- ![[Pasted image 20251211133038.png]]
## Benchmarks and Trust-Hub
- Trust-Hub provides a public repository of >100 Trojan benchmarks spanning different insertion points, activations, and effects, used by hundreds of papers as a common evaluation platform
- These benchmarks support fair comparison of detection/prevention methods, characterization of hardness, and reproducible experiments across academia and industry
## Machine Learning Based Detection
- ML is attractive because HT detection must scale to huge, complex ICs and subtle Trojans; features derived from netlists, layouts, timing, and side-channels data can be fed to regression, deep neural networks, GNNs, and reinforcement learning models
	- ![[Pasted image 20251211133520.png]]
	- ![[Pasted image 20251211133531.png]]
- ML-assisted HT detection (ML-HTD) flows integrate statistical modeling (IR-aware STA) with test-time measurements and use regressors (MLP, random forest, gradient boosting, stacking) to learn expected path slacks and flag anomalous ones as potential Trojan payloads or triggers
	- ![[Pasted image 20251211133540.png]]
	- ![[Pasted image 20251211133545.png]]
	- ![[Pasted image 20251211133600.png]]
	- ![[Pasted image 20251211133609.png]]
	- ![[Pasted image 20251211133613.png]]
	- ![[Pasted image 20251211133618.png]]
- GNN-based approaches convert RTL or gate-level netlists into dataflow graphs and train graph convolutional models to classify designs as Trojan-free or infected, with higher accuracy at RTL (smaller graphs, more semantic structure) than at gate level
	- ![[Pasted image 20251211133635.png]]
	- ![[Pasted image 20251211133640.png]]
	- ![[Pasted image 20251211133645.png]]
## Research Trends
- Early work (2007–2013) focused on classical detection (logic testing, side‑channel) and design‑for‑security methods, while later years saw growing emphasis on runtime monitors and especially ML‑based countermeasures.
- Surveys categorize ML‑for‑HT work by algorithms (e.g., DNNs, GNNs, ensembles), features (timing, power, structural metrics), and benchmarks used, highlighting that no single method covers all Trojan types, reinforcing the need for multi‑layer defenses.
# Side-Channel Attacks
## Motivation and Threat Model
- Classical crypto assumes black-box algorithms and focuses on mathematical and strength, but embedded cryptographic devices expose physical leakage (power, EM, timing, faults, etc.) that attackers can observe or induce to recover keys. 
- Devices like smart cards, FPGAs, and microcontroller-based systems operate in hostile environments where even legitimate users may have full physical access, making secure implementation as critical as secure algorithms
## Side-channel Leakage and Attack Classes
- ![[Pasted image 20251211140607.png]]
- Common side-channel outputs: power consumption, EM emissions, optical emissions, timing/delay, and acoustic emissions; common inputs for fault-style attacks include supply glitches, clock/temperature variations, light and radiation
	- ![[Pasted image 20251211140923.png]]
	- ![[Pasted image 20251211140930.png]]
- Attacks are classified as invasive vs non-invasive, active (fault injection) vs passive (observation only), and simple vs differential, depending on physical access and whether they directly interpret traces or use statistical correlation to key-dependent leakage. 
	- ![[Pasted image 20251211140944.png]]
	- ![[Pasted image 20251211140949.png]]
## Power Analysis: SPA, DPA, CPA
- Power is measured as instantaneous current over time (traces), leveraging CMOS dynamic power dependence on data and transitions; measurement setups can be built with inexpensive oscilloscopes and amplifiers
	- ![[Pasted image 20251211141249.png]]
	- ![[Pasted image 20251211141310.png]]
	- ![[Pasted image 20251211141339.png]]
	- 
- Simple Power Analysis (SPA) inspects single traces to infer control flow, operations, and sometimes key bits, especially when implementations use key-dependent branches or different instruction sequences (DES rounds, square-and-multiply)
	- ![[Pasted image 20251211141352.png]]
- Differential Power Analysis (DPA) and Correlation Power Analysis (CPA) follow a divide-and-conquer strategy; guess small key chunks, build a leakage model (Hamming weight of S-box output in AES/DES), partition traces by predicted bit or correlate model vs traces, and select the key hypothesis with the strongest differential spike or correlation; measurement-to-disclosure (MTD) quantified how many traces are needed 
	- ![[Pasted image 20251211141411.png]]
	- ![[Pasted image 20251211141418.png]]
	- ![[Pasted image 20251211141432.png]]
	- ![[Pasted image 20251211141439.png]]
	- ![[Pasted image 20251211141446.png]]
	- ![[Pasted image 20251211141452.png]]
	- ![[Pasted image 20251211141512.png]]
	- ![[Pasted image 20251211141522.png]]
## Timing and EM Attacks
- ![[Pasted image 20251211141658.png]]
- Timing attacks exploit data-dependent execution time (square-and-multiply RSA) by correlating precise runtimes with guessed exponent bits, iteratively refining key estimated as Kocher's original observation
	- ![[Pasted image 20251211141719.png]]
	- ![[Pasted image 20251211141919.png]]
- EM Analysis (EMA) uses small inductive probes to map local fields over the die, providing spatially localized, data-correlated leakage that can bypass supply smoothing and some hiding countermeasures, at the cost of more complex, noisy, and often decapsulated setups
	- ![[Pasted image 20251211141926.png]]
	- ![[Pasted image 20251211141936.png]]
	- ![[Pasted image 20251211141948.png]]
	- ![[Pasted image 20251211141954.png]]
	- ![[Pasted image 20251211142001.png]]
## Targets: Smart Cards, FPGAs, Microcontrollers
- Smart cards have simple, known architectures (8-bit CPU, external clock/power, limited RAM/ROM/EEPROM) and are classic targets for SPA/DPA on payment and identification applications
- FPGAs offer parallelism and complex, less-known structures, making visual SPA harder and pushing towards DPA/CPA and EM-based analysis to infer keys or internal configuration
- On microcontrollers, leakage stems from memory loads/stores, arithmetic, and control-flow instructions; typical attack setups send plaintexts from A PC to an on-chip core (PowerPC in FPGA), capture power traces, and apply CPA with algorithm and implementation-specific leakage models
## General Countermeasures 
- Hiding reduces signal-to-noise ratio by lowering signal (balanced/dual-rail/asynchronous logic, low-power design) or adding noise (noise generators, random activity, shielding, active emitting grids)
- Masking/blinding randomizes sensitive intermediates so leakage depends on masked values instead of raw secrets, breaking simple correlation between data and traces
- Design partitioning physically separates plaintext and key-processing regions from ciphertext and I/O regions to limit leakage and coupling of sensitive nets
- Physical security and anti-tampering restrict proximity and access (shields, tamper sensors, fault detection), making invasive/fault attacks harder or self-destructive
## Anti-SPA / Anti-DPA Specifics
- To resist SPA, implementations must avoid control-flow or microarchitectural behavior (branches, table lookups, early exits) directly dependent on secret data or keys, like constant-time algorithms and key-agnostic instruction sequences
- Anti-DPA techniques decorrelate power from data or desynchronize traces: randomized clock phases, jitter, dummy operations, shuffled execution order, and protocol-level blinding of inputs/keys
## Microcontroller-level Countermeasures
- Algorithm-level: transform C/Assembly so dangerous leakage patterns are removed or masked, including randomization of intermediates and constant-time coding practices 
- Architecture-level: implement special circuit techniques like dual-rail precharge (DRP) logic, where direct and complementary data paths switch together to yield data-independent power, and virtual secure circuits (VSC), which emulate DRP behavior in software by executing complementary operations so combined power stays approximately consistent
# Fault Injection Attacks
## Definition and Threat Landscape
- Fault injection attacks deliberately induce errors (logical or parametric) in hardware to bypass security checks, reveal secrets, or weaken cryptographic algorithms
- Adversaries range from "skilled outsiders" (cheap gear, exploit known weaknesses) throuhg knowledgeable insiders to fully funded organizations capable of semi-invasive and invasive silicon-level manipulation
## Taxonomy and Techniques
- Non-invasive methods require no physical modification: clock glitching and voltage glitching (global, timing-critical) plus EM-based techniques to corrupt control logic, memory reads, or security fuses
	- ![[Pasted image 20251211144745.png]]
	- ![[Pasted image 20251211144750.png]]
- Semi-invasive attacks (backside decap, local heating, glash/laser glitching) use high-power or IR light to locally change $V_{TH}$ or create current pulses, enabling precise bit flips in SRAM/registers without permanent damage
	- Backside decapsulation with a Dremel and polishing, IR backside imaging to reveal transistor gates, OBIC/LIVA laser scanning to distinguish “1” vs “0” in memory cells, and using that information to locate buses, control logic, and security fuses before launching precise local‑heating, flash‑glitching, or laser‑glitching faults.
	- ![[Pasted image 20251211144757.png]]
- Invasive attacks rely on Focused Ion Beam (FIB), laser cutting, and micro-probing: reverse engineer layout, edit nets, create probe points, eavesdrop or inject signals, and perform highly localized transient or permanent fault injection
	- ![[Pasted image 20251211145023.png]]
## Example Attacks
- Clock/voltage glitching on control FSMs (AES Controller) can cause illegal transitions directly into "protected" states like the final round, skipping intermediate rounds and leaking key-dependent intermediate results
	- ![[Pasted image 20251211145202.png]]
- Optical fault injection can flip individual SRAM bits by briefly opening specific NMOS devices in a 6T cell, enabling controlled corruption of key bits or code, despite diffraction limits in advanced nodes
	- ![[Pasted image 20251211145215.png]]
- "Bump" attacks on flash/EEPROM use backside decaps, IR imaging, side-channel reverse engineering of verify, and laser glitching to mask all but a few bits per operation, reducing brute-force complexity from $2^n$ to roughly $(n / \text{bus\_width}) * 2^{\text{bus\_width}}$    
- Black‑box attacks: brute force (verify‑only memories, key guessing, ciphertext‑to‑plaintext guessing), software exploits (undocumented test modes, buggy security functions, insecure interfaces), and data remanence (cooling to around −20 C so SRAM retains key data long enough to power‑cycle and read it back with custom firmware).
## FPGA Bitstream Fault Injection (BIF)
- The bitstream fault injection (BiFI) section explains design‑independent manipulation rules on LUT configuration bits so that many instances of the same Boolean function (e.g., S‑boxes) are corrupted simultaneously, generating exploitable faulty ciphertexts without knowing the RTL/netlist.
- The BiFi attack systematically flips configuration bits (especially in LUT truth-table entries) in an unknown AES design, collects faulty ciphertexts, and uses DFA-style analysis on hypothesized intermediate values to recover secret key
	- ![[Pasted image 20251211145827.png]]
	- ![[Pasted image 20251211145843.png]]
- Experiments on 16 AES cores in a Spartan-6 showed 15 were breakable, often with a single faulty ciphertext; bitstream encryption raises the bar but does not fully eliminate risk without strong authentication and on-chip integrity checks.
## Countermeasures: Intrusion and Error Detection
- Intrusion prevention/detection: complex 3D/stacked packages, front-side active shields, backside tamper-detection coatings, and analog sensors (voltage, clock, temperature, light) can raise effort or trigger zeroization on tampering, though reliability degrades in advanced nodes
- Error detection: redundancy (spatial or temporal), comparators, and error-detecting codes (EDC) can detect many injected faults, but coverage is incomplete and the detection logic and comparators themselves are fault-attack surfaces
## Broader Exploit and Mitigation Themes
- Manu fault-assisted attacks combine multiple vectors: black-box brute force, timing/power side-channels, glitching, and invasive techniques, especially against controllers/FSMs in security-critical designs like SHA-256 or AES
	- There is a specific emphasis on “fault‑injection in controller designs”: control circuits/FSMs contain protected states, so any unintended transition (e.g., into a “done” or privileged state) can compromise whole‑system security.
	- For SHA‑256, flipping a few control or state bits (changing round counters or branch conditions such as 01 --> 11) can create exploitable behavior or weaken integrity, hence FSMs in crypto controllers must be hardened against faults and Trojans.
- Defensive guidelines include: strong software QA and constant-time code, avoiding piecewise verify responses, immediate erasure of volatile memory on power-up or abnormal temperature, on-chip oscillators for security paths, brownout resets, noisy/flattened power, profiles, metal shielding, small feature sizes, and pervasive tamper sensors around memory fuses and control logic. 
# Watermarking of HW IPs
## HW IPs, Piracy, and Secure IP Goals
- HW IPs (soft RTL, firm placed netlists, hard GDSII/layout) are reused in SoCs and FPGAs and are exposed to IP overuse, cloning, modification, reverse engineering, and overproduction by integrators and foundries
	- ![[Pasted image 20251211153743.png]]
- Secure IP approaches are either preventative (obfuscation, bitstream encryption, one-time or NVM FPGAs, self-destruct in military chips, periodic licensing) or detective (watermarking, secret tagging) with design goals of strong robustness vs low overhead and a tradeoff between pre vs post processing. 
	- ![[Pasted image 20251211153806.png]]
	- ![[Pasted image 20251211153819.png]]
	- 
## Watermarking Basics and Principles
- IP watermarking gives each IP a hidden, unique signature (unlike digital media watermarking, it must be noninvasive: no functional change, negligible performance impact)
	- ![[Pasted image 20251211153937.png]]
	- ![[Pasted image 20251211153942.png]]
- Core principles: do not alter functionality, keep performance degradation negligible, make the watermark hard to detect/remove, and achieve a provably low probability of accidental authorship
	- ![[Pasted image 20251211154010.png]]
## Additive and Constraint-Based Watermarking
- Additive watermarking inserts extra logic or test structures at the source/RTL or netlist; it is flexible and strong but can increase area and power, and sometimes affect timing
- Constraint-based watermarking encodes an encrypted signature as additional design constraints (SAT clauses, CLB usage patters, path timing constraints) and lets EDA tools produce a legal implementation that simultaneously satisfies functionality and watermark constraints; CLB-based schemes are easy to find/remove but low-overhead, while timing-based schemes are much harder to detect with minimal overhead
	- ![[Pasted image 20251211155427.png]]
	- ![[Pasted image 20251211155432.png]]
## Specific Embedding Methods
- Don't-care watermarking: exploits unused input combinations or don't-care FSM transitions and forces outputs/paths to encode bits of the watermark without affecting specified behavior:
	- ![[Pasted image 20251211155529.png]]
- Test-based watermarking: embeds signatures insto scan/test structures or test sequences )ad headers, pseudorandom insertion, XOR with test bits); very robust but can carry notable area and power cost
	- ![[Pasted image 20251211155609.png]]
- Power-Signature (Side Channel) watermarking: adds logic that draws power at a specific non-harmonic frequencies so the watermark appears as spectral lines on the supply current; very hard to remove but expensive in power and area
	- ![[Pasted image 20251211155645.png]]
- Place-and-route watermarking: constrains cell placement (regions, rows, groupings) so the physical pattern encodes a signature; offers negligible area overhead and string proofs (chance of accidental pattern $\approx 1/2^N$ ) but applies mainly to firm/hard IP.
## Higher-level FSM-based watermarking
- DSP/algorithm-level watermarking tweaks filter or DSP requirements (frequency response in dB) so that the resulting coefficients encode a codework; this is inserted at algorithm level and becomes hard to detect at lower implementation levels.
	- ![[Pasted image 20251211161535.png]]
- FSM-based watermarking modifies the state transition graph: state-based schemes add states/encodings, while transition-based schemes add or reuse rare transitions; extraction relies on tools that reconstruct the FSM from RTL/gate-level designs
	- ![[Pasted image 20251211161548.png]]
## Attacks and Defenses on Watermarks
- Typical attacks include "ghost signatures" (claim coincidental patterns as own mark), tampering (removing/altering marks via reverse engineering earlier design stages), and forging (watermarking others' IP with the owner's mark by stealing keys)
- Defenses: watermark obfuscation (hide the structure), multiple small watermarks distributed across the design, and parity/redundant encoding in watermarks to detect and correct partial tampering; cryptographic protection of watermark keys prevents forging
## ActiWate: Active Integration-time Watermark
- ![[Pasted image 20251211161810.png]]
- ActiWate is an "active" watermark for 3P IPs: the IP's FSM is extended with pre-functional "pinging" states that query SoC peripherals using secret addresses and challenge-response pairs; only when all responses match expected values does the IP enter normal functional mode.
	- ![[Pasted image 20251211161950.png]]
	- ![[Pasted image 20251211161957.png]]
- This binds the IP instance to a particular SoC integration, resists rogue integrators (who must reveal peripheral info and cannot easily bypass the FSM), and avoids post-silicon extraction by using in-system behavior as proof of legitimate integration and ownership
	- ![[Pasted image 20251211162003.png]]
# FPGA Overview
## What are FPGAs
- An FPGA is an integrated circuit with a programmable array of logic and interconnect, plus DSP blocks and embedded memories, that can be configured in the field to implement many different digital circuits without new silicon
- FPGAs are widely used in automotive, defense, healthcare, networking, and IoT because they reduce time‑to‑market and NRE versus ASICs while offering better energy efficiency than general‑purpose processors
## FPGA vs ASIC
- Compared with ASICs, FPGAs are reconfigurable, cheaper and faster to iterate, reprogrammable (often with partial reconfiguration), but have higher per‑unit cost in high volume, larger form factor, higher power, and longer compile/configuration times
- Main architectural style is “island” based: arrays of logic blocks (CLBs) containing LUTs and flip‑flops, programmable routing channels, and I/O blocks at the periphery; LUTs implement small truth tables from SRAM and can be used in combinational, registered, or feedback modes
	- ![[Pasted image 20251211162755.png]]
	- ![[Pasted image 20251211162800.png]]
	- ![[Pasted image 20251211162805.png]]
	- ![[Pasted image 20251211162811.png]]
	- ![[Pasted image 20251211162819.png]]
	- ![[Pasted image 20251211162827.png]]
	- ![[Pasted image 20251211162832.png]]
	- ![[Pasted image 20251211162839.png]]
	- 
## FPGA Types
- Antifuse FPGAs are one‑time programmable: programming uses high voltage to permanently short antifuses between metal layers, giving small area, low resistance, rad‑hard, “instant‑on” behavior, but no reconfiguration
	- ![[Pasted image 20251211162850.png]]
	- ![[Pasted image 20251211162909.png]]
- Memory‑based FPGAs dominate the market:
	- Flash FPGAs are non‑volatile and power‑up configured but have limited reprogram cycles, non‑standard processes, slower and more expensive configuration.
		- ![[Pasted image 20251211162924.png]]
	- SRAM FPGAs store configuration in 6T SRAM cells controlling routing and LUT contents; they are effectively infinitely reprogrammable with fast reconfiguration and standard CMOS, but volatile, larger and leakier, and introduce security issues around the external configuration bitstream.
		- ![[Pasted image 20251211162929.png]]
- ![[Pasted image 20251211162937.png]]
## EDA Flow for FPGA Design
- ![[Pasted image 20251211162954.png]]
- Design flow steps: design conception (requirements and architecture), design entry (schematic or HDL), logic synthesis (netlist generation, optimization, and FPGA technology mapping), functional simulation (zero-delay behavioral correctness), physical design (placement and routing), static timing analysis, timing simulation, and finally configuration by loading the generated bitstream into the FPGA
	- ![[Pasted image 20251211163248.png]]
	- ![[Pasted image 20251211163257.png]]
	- ![[Pasted image 20251211163302.png]]
	- ![[Pasted image 20251211163306.png]]
	- ![[Pasted image 20251211163313.png]]
	- ![[Pasted image 20251211163317.png]]
- Placement is NP-complete, so tools use heuristics to choose CLB locations; routing then connects blocks with on-chip wires via global/detailed routing, and STA reports max clock frequency, setup/hold times, and slack, with positive slack indicating timing closure
	- KLFM, Circuit Partitioning, Maze Routing, Pathfinder, Versatile Place and Route (reconfig knowledge)
## Timing and Tools
- Timing simulation and STA consider both gate and interconnect delays and are implemented using commercial tools such as PrimeTime, Tempus, or RedHawk to verify timing margins on the post‑layout design
# EDA Tools
## Platform and Cost Tradeoffs
- Early in design, specifications cover performance, power, cost, and target platform (uP, FPGA/CPLD, or ASIC), since this choice dominates NRE, unit cost, and schedule
- FPGAs have low NRE but high per-unit cost, so they are preferable at low volume; ASICs have very high NRE but low per-unit cost for high volume; microprocessors often remain cheapest overall across volumes when performance/energy are acceptable
	- ![[Pasted image 20251211163911.png]]
## High-Level Digital Design vs Analog
- Analog ICs are usually semi/full custom: transistor geometries and interconnects are manually crafted for exact performance but do not scale to large gate counts. 
	- ![[Pasted image 20251211164048.png]]
- Digital ASICs rely on characterized standard-cell libraries and EDA tools for synthesis, place and route, enabling million-gate designs at the expense of some performance/area optimality.
## Core ASIC Design Flow and Key Tools
- RTL Stage: write HFL, define interfaces, partition into blocks (datapath, control, memories), simulate functionality with testbenches, and debug until behavior matches spec.
- Synthesis: use tools like Design Compiler to translate RTL to gate-level netlist, applying design constraints (sdc, xdc) for timing, area, and power and optionally inserting scan DFT structures
- Physical design: IC Compiler/Encounter do floor planning, power grid, placement, clock-tree synthesis, and routing; post-route parasitics are extracted and STA plus physical verification (DRC/LVS via Calibre, etc.) are run before GDSII handoff to the foundry
	- ![[Pasted image 20251211164403.png]]
	- ![[Pasted image 20251211164408.png]]
	- ![[Pasted image 20251211164414.png]]
	- ![[Pasted image 20251211164419.png]]
	- ![[Pasted image 20251211164426.png]]
	- ![[Pasted image 20251211164430.png]]
	- ![[Pasted image 20251211164435.png]]
	- ![[Pasted image 20251211164439.png]]
	- ![[Pasted image 20251211164444.png]]
	- ![[Pasted image 20251211164448.png]]
	- ![[Pasted image 20251211164452.png]]
	- ![[Pasted image 20251211164456.png]]
## Testing, Fabrication, and Packaging
- Some photos up top
- Manufacturing defects require VLSI testing to separate bad dies: structural test with scan FFs and ATGP (like TetraMax) covers stuck-at and related faults, and BIST can be used on-chip
- After GDSII, the foundry generates masks, runs a multi-hundred step lithography flow on wafers, then performs wafer-level and die-level tests using ATE before dicing, packaging (wire bonding, BGA/DIP, etc.), board assembly, and shipment
## Classes of EDA Tools and Security Applications
- EDA Tool Classes:
	- Design entry (Vivado editors), implementation (Design Compiler, IC Compiler, scan insertion), and validation (VCS, IFV, PrimeTime, Calibre, Voltus)
- In HW Sec, these tools support
	- VCS for logic-obfuscation evaluation and Trojan detection via output corruption and switching-activity analysis
	- IFV (formal) to derive Trojan activation sequences and debug assertions
	- Design Compiler to estimate switching and timing, choose key-gate insertion points and edit netlists for obfuscation
	- TetraMax to generate patters that reveal obfuscation keys or identify low-controllability/observability Trojan nets