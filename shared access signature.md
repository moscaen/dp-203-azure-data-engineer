# SAS

- An account SAS, introduced with version 2015-04-05. This type of SAS delegates access to resources in one or more of the storage services. All of the operations available via a service SAS are also available via an account SAS.

    With the account SAS, you can delegate access to operations that apply to a service, such as Get/Set Service Properties and Get Service Stats. You can also delegate access to read, write, and delete operations on blob containers, tables, queues, and file shares that are not permitted with a service SAS.

- A service SAS. This type of SAS delegates access to a resource in just one of the storage services: Azure Blob Storage, Azure Queue Storage, Azure Table Storage, or Azure Files. For more information, see Create a service SAS and Service SAS examples.

- A user delegation SAS, introduced with version 2018-11-09. This type of SAS is secured with Azure Active Directory credentials. It's supported for Blob Storage only, and you can use it to grant access to containers and blobs. For more information, see Create a user delegation SAS.

<table aria-label="Signing a SAS token with an account key" class="table table-sm">
<thead>
<tr>
<th>Type of SAS</th>
<th>Type of authorization</th>
</tr>
</thead>
<tbody>
<tr>
<td>User delegation SAS (Blob storage only)</td>
<td>Azure AD</td>
</tr>
<tr>
<td>Service SAS</td>
<td>Shared Key</td>
</tr>
<tr>
<td>Account SAS</td>
<td>Shared Key</td>
</tr>
</tbody>
</table>
