# Theory of Operation
* Goals
	* Prepare for CCDC and Collegiate Pen Testing Comp and others like DoE Cyberforce and NCAE Cyber Games
	* Prepare students for adversarial cyber engagements in life Blue/Red/Purple Team. CTFs
* Grading
	* 20% Quizzes
	* 40% Activity Reports
	* 40% Final Exam (Format TBD, prob a practical examination)
* CIAAAN
	* Confidentiality - access to information shall be granted to only those agents authorized to have such access
	* Integrity - information shall not be modified in ways that are not intended by authorized agents
	* Availability - information that should be accessible to authorized agents must be accessible to those agents
	* Authentication - one must be able to correctly establish an agent's identity
	* Authorization - an agent's access must be appropriately constrained based on a policy that governs such access
	* Nonrepudiation - any record of an event must be preserved in such a way that its occurrence cannot be denied
# Game Theory
* Game theory is a mathematical discipline that models strategic interactions. Seminal work in this area was done by von Neumann and later Nash
* One important aspect of game theory is providing a method of determining a *best response* of a player in any given situation
* Three inputs:
	* Players
	* Available information and actions
	* Consequences of decisions
* Goal is removal of control (CIAAAN attributes) from your opponent
* Shifting strategies yield reaction correspondences, or best responses to opponent's actions
* The Nash Equilibrium is reached when each of the players engages in optimal play
* Borges (textbook author) points out that computer security is probably complicated enough not to exhibit a Nash Equilibrium
* Even is such equilibrium exists, current performance of both red and blue team actors at its best is surely far from optimal.
* Resource limits make it even more difficult to react optimally
* Good teams:
	* Review each other's work
	* Analyze which strategies outperform others
	* Study emerging techniques
	* Adapt well to a changing landscape
# Principles of Computer Conflict
* Defense in depth - wear suspenders and a belt to keep your pants from falling down!
* Use kill chain models - characterize successful attack patterns and allow defenders to prepare interdictions
* Attack Trees [here](https://www.cise.ufl.edu/~jnw/Tradecraft/AttackTrees.pdf) - provide a way to visually describe the elements of a kill chain
* Borges uses the [Mitre ATT&CK Framework](https://mitre-attack.github.io/attack-navigator/) to describe attacks
* Offense vs Defense
	* Computer security is an asymmetric game because different technologies, skills, and strategies are optimal for the opposing players
	* Computer security is *not* a zero-sum game, that is, a game in which no wealth is created or destroyed. A player's business model may drive them to value elements of CIAAAN differently
	* The defense depends largely on monitoring and logging to identify attacks
	* The offense uses guile and deception to gain an advantage over the defense
	* Purple teaming is what we are trying to do- working together to help the blue team become better
	* Offense may take either a *low-and-slow* approach or *shock-and-awe*. CCDC and CPTC are much different from CTFs and employ different tools and mindsets
	* Offensive goal: persist undetected while exploiting resources to maximum advantage
# Principles to be employed in this class
* Principle of Deception - all warfare is based on deception
* Physical Access - the defender usually has physical access to assets
* Humanity - attackers and defenders are people
* Economy - everyone has limited resources
* Planning - The 5 Ps. Complexity is your enemy- simplify through planning
* Innovation - research, required time and talent to create
* Time - timing is everything