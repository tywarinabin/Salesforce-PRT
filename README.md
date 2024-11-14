# HarvestCRM

### **Project Overview**
HarvestCRM is a CRM solution built to manage the daily operations of a flour mill factory. This application tracks flour production, sales, and inventory, generating daily reports automatically for the owner. HarvestCRM enhances customer relationship management and improves business efficiency by automating data collection, reporting, and communication.

### **Objectives**

#### **Business Goals**
- **Enhance Customer Retention**: Improve relationships with automated messaging to encourage repeat purchases.
- **Optimize Resource Distribution**: Efficiently allocate flour to minimize waste.
- **Improve Payment Processing**: Streamline financial management with integrated payment tracking.
- **Enable Data-Driven Decisions**: Provide real-time reports and dashboards for strategic planning.

#### **Specific Outcomes**
- **Effective Consumer Communication**: Automate personalized messages to build customer trust.
- **Improved Efficiency**: Role-based access for Owner, Employer, and Worker ensures secure, efficient operations.
- **Enhanced Reporting and Analytics**: Generate real-time insights on sales and inventory.
- **Seamless Consumer Tracking**: Accurately track distribution, payments, and customer details.

### **Key Features and Concepts**

- **Custom Objects and Fields**: Supplier, Flour Mill, Consumer, and Flour Details objects to manage supply chain data.
- **Relationships**: Master-detail and lookup relationships maintain data integrity.
- **Validation Rules**: Enforces data completeness for consumer information.
- **Page Layouts and Profiles**: Tailored layouts per role with data visibility controls.
- **Reports and Dashboards**: Provides daily reports and visual insights on consumer data.
- **Apex Automation**: Automates tasks like sending welcome emails to new consumers.

### **Detailed Solution Design**

#### **1. Custom Objects and Fields**
- **Supplier Object**: Manages suppliers with fields for `Supplier Name` and `Total Flour Distributed`.
- **Flour Mill Object**: Tracks inventory and sales, with fields like `Flour Distributed` and `Flour Price per kg`.
- **Consumer Object**: Stores consumer purchase and contact information.
- **Flour Details Object**: Tracks flour distribution data from suppliers.

#### **2. Page Layouts**
- **Supplier Layout**: Displays supplier details and total flour distribution.
- **Flour Mill Layout**: Shows flour inventory and distribution data.
- **Consumer Layout**: Organizes consumer contact and purchase details.
- **Flour Details Layout**: Presents distribution data linked to supplier and mill.

#### **3. Profiles, Role Hierarchy, and User Assignment**
- **Profiles**: Custom profiles for Owner (full access), Employer (read/edit), Worker (limited read/create access).
- **Role Hierarchy**: Structured as Owner > Employer > Worker to control data visibility.
- **User Assignment**: Assigns roles and profiles based on responsibilities.

#### **4. Apex Triggers and Classes**
- **Trigger: SendWelcomeEmailToConsumer**
  - Purpose: Sends an automated welcome email to new consumers.
  - **Trigger Code**:
    ```apex
    trigger consumerTrigger on Consumer__c (after insert) {
        if (Trigger.isAfter && Trigger.isInsert) {
            ConsumerRecord.sendEmailNotification(Trigger.new);
        }
    }
    ```

  - **Apex Class**:
    ```apex
    public class ConsumerRecord {
        public static void sendEmailNotification (List<Consumer__c> consumers) {
            for (Consumer__c consumer : consumers) {
                Messaging.SingleEmailMessage email = new Messaging.SingleEmailMessage();
                email.setToAddresses(new List<String>{consumer.Email__c});
                email.setSubject('Welcome to our company');
                email.setPlainTextBody(
                    'Dear ' + consumer.First_Name_c + ' ' + consumer.Last_Name_c + ',\n\n' +
                    'Welcome to MY RICE! We value your relationship with us.\n'
                );
                Messaging.sendEmail(new List<Messaging.SingleEmailMessage>{email});
            }
        }
    }
    ```

#### **5. Reports and Dashboards**
- **Flour Mill Consumer Report**: Tracks purchases by flour type, payment method, and amount paid. Useful for understanding demand and distribution.
- **Dashboards**: Provide quick analysis with metrics on sales by flour type, consumer engagement, and payment trends.

### **Conclusion**
HarvestCRM streamlines flour mill operations, enhances consumer engagement, and optimizes inventory management. By automating workflows and providing insightful data, it supports the business in achieving long-term growth and operational efficiency.

---

### **Installation**

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/HarvestCRM.git
