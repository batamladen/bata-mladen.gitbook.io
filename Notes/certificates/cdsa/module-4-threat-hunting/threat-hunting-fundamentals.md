# Threat Hunting Fundamentals

## **Threat Hunting Definition**

Threat hunting is an active, human-led, hypothesis-driven practice that systematically examines network data to identify stealthy, advanced threats that evade traditional security measures. Unlike conventional, reactive cybersecurity methods, threat hunting proactively seeks out malicious activity before it can cause significant damage.

The primary goal of threat hunting is to reduce dwell time, which is the duration an attacker remains undetected within a network. By detecting adversaries early in the cyber kill chain, organizations can prevent them from establishing a foothold and mitigate potential harm.

### The Threat Hunting Process

1. **Asset Identification** – Recognizing high-value targets within the network.
2. **TTP Analysis** – Understanding the Tactics, Techniques, and Procedures (TTPs) used by adversaries.
3. **Detection & Isolation** – Identifying and investigating artifacts and anomalies.
4. **Threat Intelligence Integration** – Utilizing intelligence to refine hypotheses and enhance detection.

### Key Facets of Threat Hunting

* **Proactive Strategy** – Anticipating threats rather than reacting to incidents.
* **Investigative Analysis** – Searching for artifacts and anomalies that signal compromise.
* **Deep Cybersecurity Knowledge** – Understanding threats, attack patterns, and the cyber kill chain.
* **Adversarial Mindset** – Thinking like attackers to predict and counteract their moves.
* **Environmental Awareness** – Familiarity with network infrastructure and normal activity baselines.
* **Advanced Analytics & Tools** – Using high-fidelity data and sophisticated platforms for detection.

***

## **The Relationship Between Incident Handling & Threat Hunting**

Threat hunting and incident handling are intertwined processes that enhance organizational cybersecurity. Their relationship is evident in various phases of incident handling:

1. **Preparation** – Establishing protocols and integrating threat hunting into incident handling procedures.
2. **Detection & Analysis** – Assisting in incident verification and uncovering additional artifacts.
3. **Containment, Eradication, & Recovery** – Some organizations involve hunters in these phases to ensure complete threat removal.
4. **Post-Incident Activity** – Hunters contribute insights to strengthen security defenses and improve future threat detection.

Organizations must decide whether to integrate threat hunting into incident handling or treat it as an independent function based on their unique threat landscape and risk management strategy.

***

## **A Threat Hunting Team's Structure**

An effective threat hunting team comprises professionals with diverse expertise, ensuring a comprehensive approach to identifying and mitigating threats. Common roles include:

* **Threat Hunter** – Core team members specializing in threat detection and analysis.
* **Threat Intelligence Analyst** – Gathers and analyzes intelligence to guide hunting efforts.
* **Incident Responders** – Handles containment, eradication, and recovery after detecting threats.
* **Forensics Experts** – Conducts in-depth investigations and reverse-engineers attacks.
* **Data Analysts/Scientists** – Uses statistical models and machine learning for pattern recognition.
* **Security Engineers/Architects** – Designs security infrastructure and implements defensive measures.
* **Network Security Analyst** – Monitors network traffic and identifies anomalies.
* **SOC Manager** – Oversees team coordination and communication.

***

## **When Should We Hunt?**

Threat hunting should be a continuous process, but specific triggers warrant immediate action:

1. **New Threat Intelligence** – When fresh information about adversaries or vulnerabilities emerges.
2. **New IoCs for Known Adversaries** – When updated Indicators of Compromise (IoCs) are linked to known threats.
3. **Multiple Network Anomalies** – When suspicious activities occur in quick succession.
4. **During Incident Response** – Complementing IR efforts to uncover additional compromises.
5. **Regular Proactive Actions** – Conducting periodic hunts to maintain security resilience.

**Conclusion**: The best time for threat hunting is always now. Proactive threat detection minimizes risks and enhances security posture.

***

## **The Relationship Between Risk Assessment & Threat Hunting**

Risk assessment plays a crucial role in guiding threat hunting activities by identifying vulnerabilities and prioritizing security efforts.

* **Prioritization** – Directing hunts toward critical assets and high-risk areas.
* **Threat Landscape Understanding** – Using risk assessments to inform hunting hypotheses.
* **Vulnerability Identification** – Targeting known weaknesses to detect potential exploits.
* **Risk Mitigation** – Developing strategies to address detected threats effectively.

By integrating risk assessment insights into threat hunting, organizations can enhance their security operations and proactively defend against emerging cyber threats.

***

## Quiz

1. What is the primary goal of threat hunting?
2. Name two key facets of threat hunting.
3. How does threat hunting relate to incident handling?
4. List three roles in a threat hunting team.
5. When should threat hunting be conducted?
6. How does risk assessment enhance threat hunting?
7. What is an Indicator of Compromise (IoC)?
8. Why is an adversarial mindset important in threat hunting?
9. What does TTP stand for in cybersecurity?
10. What is dwell time in the context of threat hunting?

**Answers:**

1. Reduce dwell time and detect threats early.
2. Proactive strategy, investigative analysis (any two from the list).
3. It assists in detection, containment, and post-incident analysis.
4. Threat hunter, threat intelligence analyst, incident responder (any three from the list).
5. Continuously and in response to triggers like new IoCs or anomalies.
6. It helps prioritize assets and guide hunting efforts.
7. A digital artifact indicating potential compromise.
8. Helps predict and counteract attacker moves.
9. Tactics, Techniques, and Procedures.
10. The duration an attacker remains undetected in a network.
