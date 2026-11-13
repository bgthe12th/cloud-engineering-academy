
# ADR-001: Document Storage Architecture

**Status:** Proposed
**Project:** Maven Secure Driver-Document Vault
**Cloud Platform:** Microsoft Azure

## 1. Business Problem

Maven Logistics manages highly important documentation for multiple carriers, requiring adequate storage to match growing fleets, and security that prevents the sensitivity of the documents being compromised. Document storage and reliable maintenance for Maven’s carriers are crucial due to the frequent receipt and sharing of documents regarding company logistics, human resources, compliance, finances, and legal issues. The loss or lack of these important documents could lead to crippling financial, legal, and operational damage to a carrier, which is why Maven requires optimized storage design and management.   

## 2. Architecture Decision

Maven will store all documents in Azure Blob Storage, separate from the application servers. Carrier separation by container or path alone does not enforce security. The backend must verify the authenticated user's carrier membership, role, and permission to access each requested document. Storage access will use appropriately scoped permissions, and the customer-facing identity provider will be selected separately from the application's Azure managed identity.
 The approach balances security, durability, and cost. It can scale automatically with fleet growth and keep documents independent of the application’s lifecycle.

## 3. Alternatives Considered

### Alternative A: Store documents on the application VM
Advantages: Simple to build, no extra services, low initial cost.
Disadvantages: Limited disk capacity, documents are lost or unavailable if the VM fails, and scaling to multiple servers is difficult.

### Alternative B: Use Azure Blob Storage

Advantages: Virtually unlimited capacity, built-in redundancy, and considerable access controls.
Disadvantages: Added service cost and integration work.

## 4. Security Considerations

Users can authenticate through a Microsoft ID, and the application would authorize each request by the user’s carrier role. Each carrier’s documents would live in a separate container or path, so one carrier cannot access another’s files. The application would use a managed identity with least privilege. 

## 5. Reliability Considerations

Documents live outside the application server, so a server failure does not affect them. A replacement server reconnects to the same storage, and using Azure geo-redundant replication services could protect against regional outage. Separating storage from application servers improves data durability but does not independently guarantee application availability. Healthy application instances, monitoring, failover procedures, and recovery testing will be evaluated. Geographic replication protects against certain regional failures, but soft delete, versioning, and other recovery measures must be considered for accidental deletion or modification.

## 6. Costs and Trade-offs

Ongoing storage and transaction costs that grow with usage.
Additional complexity in configuring access policies and carrier isolation.
Network dependency: the application cannot retrieve documents if the storage service is unreachable.


## 7. Conclusion

Blob Storage offers the scalability, security, and durability that Maven’s carriers need. Its costs and complexity are manageable compared with the risk of losing critical documents.

## 8. Implementation Status

Design exercise only. Infrastructure has not
yet been deployed or tested.


