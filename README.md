# Rust-Raft ⚠️ (active development)

Rust Raft Implementation

![Flow Diagram](https://github.com/TheDhejavu/rust-raft/blob/main/public/rust-raft.png)

## Overview 

**rust-raft** is a rust implementation of the Raft distributed consensus protocol ([Raft Website](https://raft.github.io)).
 
### What is Raft?

Raft is a consensus algorithm designed for distributed systems. It draws inspiration from the complexity of the Paxos consensus algorithm but aims to be more straightforward and intuitive. Raft is tailored to address non-Byzantine fault tolerance issues.


### Project Details:
- Replicated State Machine and Logs:
The Rust Raft implementation is designed to replicate a state machine (FSM) across multiple nodes, leveraging the powerful capabilities of Sled(https://github.com/spacejam/sled) for stable and log storage. Sled is a modern, high-performance embedded database library for Rust. This implementation ensures that logs containing a sequence of operations remain consistent across the entire distributed system.


    ###### Log Replication flow

    ```mermaid
    sequenceDiagram
        participant Client
        participant Leader
        participant Follower1
        participant Follower2

        Client->>Leader: Client Request
        Note over Leader: Process and add to log

        Leader->>Follower1: AppendEntries 
        Note over Follower1: Check term, log consistency
        Follower1->>Leader: Success (or Failure)

        Leader->>Follower2: AppendEntries 
        Note over Follower2: Check term, log consistency
        Follower2->>Leader: Success (or Failure)

        Note over Leader: If replication quorum size is met, update commit index.

    ```

- Uses Channels for Communicating with the RaftNodeServer:
The channel functions as an API layer for interacting with the RaftNodeServer, whether it's functioning as a leader or follower. It provides a structured way for components to communicate, facilitating seamless interaction and coordination within the Raft implementation

- GRPC Communication Support for Nodes:
The Raft nodes leverage gRPC (Google Remote Procedure Call) for communication. gRPC is a high-performance, open-source RPC framework that supports various programming languages. This choice of communication framework provides a robust and efficient layer for nodes to exchange messages and execute remote procedures, a critical aspect of implementing the Raft protocol.

- Supports Single-Server Cluster Membership Changes and Leadership Transfer:
The Raft implementation boasts the capability to dynamically handle changes in cluster membership. This includes scenarios such as adding or removing nodes from the cluster. Additionally, the system supports leadership transfer, allowing a node to gracefully hand over leadership to another node when necessary. 

- Basic K-V Distributed Database in `/examples` for Executing Multiple Write-Requests on the Leader:
As a practical example, the project includes a basic Key-Value distributed database located in the `/examples`` directory. This database serves to showcase the Raft implementation's ability to execute multiple write requests concurrently on the Raft leader node. This demonstration emphasizes the practical application of Raft, highlighting its capacity to handle concurrent writes, maintain consistency, and ensure data integrity across the distributed system.


## Raft Consensus Algorithm Features

- [x] Leader Election
- [x] Log Replication
- [x] Cluster Membership Changes
- [ ] Snapshotting / Log Compaction


## Resources
- CONSENSUS: BRIDGING THEORY AND PRACTICE [https://web.stanford.edu/~ouster/cgi-bin/papers/OngaroPhD.pdf]
- In Search of an Understandable Consensus Algorithm [https://raft.github.io/raft.pdf]
- Hashicorp Raft [https://github.com/hashicorp/raft]

Feel free to explore and contribute to this Raft implementation in Rust. For more details on Raft, refer to the [official Raft website](https://raft.github.io).
