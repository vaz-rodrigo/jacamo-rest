
jacamo-web (formerly jacamo-rest) is an interactive programming IDE based on JaCaMo (Jason + CArtAgO + Moise) Multi-Agent Oriented Programming platform. The 

Interactive Programming with jacamo-web:
![Alt text](https://g.gravizo.com/source/programmingconcept?https%3A%2F%2Fraw.githubusercontent.com%2Fjacamo-lang%2Fjacamo-rest%2Fmaster%2FREADME.md)
<details> 
<summary>Interactive programming concept with jacamo-web</summary>
programmingconcept
digraph G {
	graph [
		rankdir="RL"
	]
	subgraph cluster_1 {
		label="Environment: CArtAgO";
		Artifact [shape = record, label="Artifact"];
		ArtInsp [shape = plain, label="Artifact Inspection"];
		ArtEdit [shape = plain, label="Edit artifact code"];
		ArtCreate [shape = plain, label="Make new artifact"];
		ArtDispo [shape = plain, label="Dispose artifact"];
	}
	subgraph cluster_0 {
		label="Agents: Jason";
		Agent [label="Agent"];
		AgInsp [shape = plain, label="Agent Inspection"];
		AgCmd [shape = plain, label="Send commands"];
		AgEdit [shape = plain, label="Edit Agent"];
		AgCreate [shape = plain, label="Create agent"];
		AgKill [shape = plain, label="Kill agents"];
		AgDF [shape = plain, label="Directory Facilitator"];
	}
	subgraph cluster_2 {
		label="Organisation: Moise";
		Org [shape = tab, label="Organisation"];
		OrgInsp [shape = plain, label="Organisation Inspection"];
		OrgEdit [shape = plain, label="Edit organisation"];
		OrgAgR [shape = plain, label="Adopting role"];
		OrgAgM [shape = plain, label="Commiting mission"];
	}
	AgInsp -> Agent [color = gray20, fontcolor = gray20, style = dotted];
	AgCmd -> Agent [color = gray20, fontcolor = gray20, style = dotted];
	AgEdit -> Agent [color = gray20, fontcolor = gray20, style = dotted];
	AgCreate -> Agent [color = gray20, fontcolor = gray20, style = dotted];
	AgKill -> Agent [color = gray20, fontcolor = gray20, style = dotted];
	AgDF -> Agent [color = gray20, fontcolor = gray20, style = dotted];
	ArtInsp -> Artifact [color = gray20, fontcolor = gray20, style = dotted];
	ArtEdit -> Artifact [color = gray20, fontcolor = gray20, style = dotted];
 	ArtCreate -> Artifact [color = gray20, fontcolor = gray20, style = dotted];
 	ArtDispo -> Artifact [color = gray20, fontcolor = gray20, style = dotted];
 	OrgInsp -> Org [color = gray20, fontcolor = gray20, style = dotted];
	OrgEdit -> Org [color = gray20, fontcolor = gray20, style = dotted];
	OrgAgR -> Org [color = gray20, fontcolor = gray20, style = dotted];
	OrgAgM -> Org [color = gray20, fontcolor = gray20, style = dotted];
}
programmingconcept
</details>

To run:
* `gradle marcos` runs agent marcos and the REST platform
* `gradle bob` runs agents bob and alice. Bob sends a message to marcos using its rest API.
* see http://yourIP:8080 for a web interface (see the console for the right IP)

With docker:
* `docker build  -t jomifred/jacamo-runrest .` to build a docker image
* `docker network create jcm_net`
* `docker run -ti --rm --net jcm_net  --name host1 -v "$(pwd)":/app jomifred/jacamo-runrest gradle marcos` to run marcos.jcm
* `docker run -ti --rm --net jcm_net  --name host2 -v "$(pwd)":/app jomifred/jacamo-runrest gradle bob_d` to run bob.jcm

Notes:
* Each agent has a REST API to receive message and be inspected
* ZooKeeper is used for name service. Bob can send a message to `marcos` using its name. ZooKeeper maps the name to a URL.
* DF service is provided also by ZooKeeper
* Java JAX-RS is used for the API

See ClientTest.java for an example of Java client. It can be run with `gradle test1`.

# REST API

## for humans

* GET HTML `/agents`:
    returns the list of running agents

* GET HTML `/agents/{agentname}/mind`
    returns the mind state of the agent

* GET HTML `/services`
    returns the DF state

## for machine (or intelligent humans)

* POST `/agents/{agentname}`
    creates a new agent.

* DELETE `/agents/{agentname}`
    Kills the agent.

* POST XML `/agents/{agentname}/mb`
    Adds a message in the agent's mailbox. See class Message.java for details of the fields.

* GET XML `/agents/{agentname}/mind`
    returns the mind state of the agent

* GET TXT `/agents/{agentname}/plans`
    returns the agent's plans. A label can be used as argument:
    `/agents/{agentname}/plans?label=planT`

* POST FORM `/agents/{agentname}/plans`
    upload some plans into the agent's plan library


See RestImpl.java for more
