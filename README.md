# Gameserver-Orchestration-with-K8S
## General Idea
A K8S control plane constantly running from some low TPU device together with one or more old gaming computers working as worker nodes. Since running a gaming computer on idle is relatively expensive in terms of power (and are annoying to listen to), the system should be robust to adding and removing nodes on the fly. The control plane itself (in case it's tainted) or another low TPU device should host a webpage from which users can see available worker nodes and delegate containerized game servers to them. Some persistent storage should be set up in the system to save game progress. Setting up the system should be handled by running ansible-playbooks.

## End Goal
- [ ] A worker node is started up in some location, it runs a startup script that connects it to the cluster. 
    - [ ] Ansible playbook to setup the node initially
    - [ ] A startup script that connects to node to the cluster
- [ ] The webpage shows that a new node is available to service game servers. 
    - [ ] A webpage that hooks into the control plane API server
- [ ] Clicking on the newly added node reveals a list of possible game servers that can be picked from.
    - [ ] The visual elements needed for the webpage to function
- [ ] Choosing a game changes the node to visually indicate that a game is running on it.
- [ ] Upon successfully starting the game server the node should show an IP which can be used to connect.
- [ ] (optional) The CPU & RAM usage is logged, and are used to denote how many unique game servers can run on a single node.
    - [ ] Setting up system logging
