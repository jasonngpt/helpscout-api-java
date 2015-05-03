Help Scout Java Wrapper
=======================
Forked Java Wrapper for the Help Scout API. More information on our developer site: [http://developer.helpscout.net](http://developer.helpscout.net).

See the [Changelog](https://github.com/jasonngpt/helpscout-api-java/blob/master/CHANGELOG.md) for details.

Requirements
---------------------
* JRE 1.6

Example Usage: API
---------------------
```java
import net.helpscout.api.*;

import java.util.ArrayList;
import java.util.Collection;
import java.util.List;

public class TestingAPI {

  public static void main(String[] args) throws ApiException {
  	ApiClient client = ApiClient.getInstance();
    client.setKey("your-api-key-here");

	// Get mailboxes
  	List<String> fields = new ArrayList<String>();
  	fields.add("name");
	fields.add("email");
	fields.add("id");
	Page mailboxes = client.getMailboxes(fields);
	if (mailboxes != null) {
		// do something
		Collection allMailboxes = mailboxes.getItems();

		for (Object mailboxObj : allMailboxes) {
			Mailbox mailbox = (Mailbox) mailboxObj;
			System.out.println("Name: " + mailbox.getName());
			System.out.println("ID: " + mailbox.getId());
		}
	}

	// Get folders
	Mailbox mailbox = client.getMailbox(85);
	if (mailbox != null) {
		String mailboxName = mailbox.getName();
		Page folders = client.getFolders(37803);

		if (folders != null) {
			Collection allFolders = folders.getItems();

			for (Object foldersObj : allFolders) {
				Folder folder = (Folder) foldersObj;
				System.out.println("Name: " + folder.getName());
			}
		}
	}

	// Get customers
	Page allCustomers = client.getCustomers(0);
	Collection customers = allCustomers.getItems();

	for (Object customersObj : customers) {
		Customer customer = (Customer) customersObj;

		System.out.println("Customer Name: " + customer.getFirstName() + " " + customer.getLastName());
		if (customer.hasSocialProfiles()) {
			List profiles = customer.getSocialProfiles();
			// Do something
		}
	}
  }
}
```

Field Selectors
---------------------
Field selectors are given as a list of Strings. When field selectors are used, the appropriate object is created with the fields provided.

ApiClient Methods
--------------------
Each method also has a duplicate that allows you to pass in a list of Strings to specify desired fields (see Field Selectors).

### Mailboxes
* getMailboxes()
* getMailbox(Integer mailboxID)

### Folders
* getFolders(Integer mailboxID)

### Conversations
* getConversationsForFolder(Integer mailboxID, Integer folderID)
* getConversationsForMailbox(Integer mailboxID)
* getConversationsForCustomerByMailbox(Integer mailboxID, Integer customerID)
* getConversation(Integer conversationID)
* createConversation(Conversation conversation)
* createConversationThread(Long conversationId, ConversationThread thread)
* updateConversation(Conversation conversation)
* deleteConversation(Long id)

### Attachments
* getAttachmentData(Integer attachmentID)
* createAttachment(Attachment attachment)
* deleteAttachment(Long id)

### Customers
* getCustomers()
* getCustomer(Integer customerID)
* createCustomer(Customer customer)
* updateCustomer(Customer customer)

### Users
* getUsers()
* getUsersForMailbox(Integer mailboxID)
* getUser(Integer userID)


Example Usage: Webhooks
------------------------
<pre><code>
Webhook webhook = new Webhook('secret-key-here', httpRequest);
if (webhook.isValid()) {
   String event = webhook.getEventType();

   if (webhook.isConversationEvent()) {
	Conversation convo = webhook.getConversation();
	// do something
   } else if (webhook.isCustomerEvent()) {
	Customer customer = webhook.getCustomer();
	// do something
   }
}
</code></pre>
