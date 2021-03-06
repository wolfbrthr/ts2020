.. _deploycca:

--------------------------------------------
Deploying Xi Frame Cloud Connector Appliance
--------------------------------------------

Introduction...

**In this lab...**

.. note::

   The Frame Cloud Connector creation process is 100% automated starting with the release of Prism Central 5.11.1 via the Settings menu. For the purposes of this lab, you will follow the manual procedure below for deploying and configuring the appliance VM.

Creating a Frame Account
++++++++++++++++++++++++

#. In a browser, log into https://my.nutanix.com with your My Nutanix credentials.

#. Under **Xi Cloud Services > Xi Frame**, click **Start Trial**.

   .. figure:: images/0a.png

#. Provide the additional requested information and click **Continue** to activate your 30 day trial.

#. When prompted with multiple trial options, click **Start your 30 day free trial**. This will allow you to provide your own infrastructure to host desktops for up to 5 named users for a 30 day period.

   .. figure:: images/0b.png

#. You will be taken to the Frame Account Administration portal. Select **Organizations** from the left hand menu and click **Add Organization**. Specify a name and save.

   .. figure:: images/0c.png

   .. note::

      If you are prompted with an authorization error, inform a proctor so that your account can be fixed.

   <Do we want to include info in here about customers/organizations/accounts?>

Adding Prism Service Account
++++++++++++++++++++++++++++

#. In **Prism Central**, select :fa:`bars` **> Prism Central Settings > Local User Management**.

#. Create a new service account for the Frame CCA but filling out the following fields and clicking **Save**:

   - **Username** - *Initials*\ -FrameSvc
   - **First Name** - *Initials* Frame
   - **Last Name** - Service Account
   - **Email** - (Any e-mail address)
   - **Password** - techX2020!
   - Under **Roles**, select **User Admin** and **Prism Central Admin**

   .. figure:: images/1.png

Adding Frame Category
+++++++++++++++++++++

<Categories are used to...>

.. note::

   Creation of the category and values only needs to occur once per Prism Central instance, but the category values will need to be assigned to the appropriate VMs for all subsequent lab completions.

#. In **Prism Central**, select :fa:`bars` **> Virtual Infrastructure > Categories**.

   .. figure:: images/2.png

#. Review the available categories. If **FrameRole** doesn't already exist, click **New Category** and fill out the following fields:

   - **Name** - FrameRole
   - **Purpose** - Allowing resource access based on Application Team
   - **Values**
   
      - Instance
      - Template
      - MasterTemplate

   .. note::

      Use the :fa:`plus` button to add additional values.

   .. figure:: images/2b.png

#. Click **Save**.

#. In **Prism Central**, select :fa:`bars` **> Virtual Infrastructure > VMs** and select your *Initials*\ **-FrameImage** VM.

#. Select **Actions > Manage Categories** and add the **FrameRole:MasterTemplate** value to the VM. The Frame CCA will later search for VMs with this category value. Click **Save**.

   .. figure:: images/2c.png

Creating the CCA VM
+++++++++++++++++++

#. In **Prism Central**, select :fa:`bars` **> Virtual Infrastructure > VMs**.

#. Click **Create VM**.

#. Select your assigned cluster and click **OK**.

#. Fill out the following fields:

   - **Name** - *Initials*-FrameCCA
   - **Description** - (Optional) Description for your VM.
   - **vCPU(s)** - 1
   - **Number of Cores per vCPU** - 2
   - **Memory** - 4 GiB

   - Beside **Disks > CD-ROM**, select :fa:`pencil`
      - **Operation** - Clone from Image Service
      - **Image** - FrameCCA-2.1.6.iso
      - Select **Update**

   - Select **+ Add New Disk**
      - **Type** - DISK
      - **Operation** - Allocate on Storage Container
      - **Storage Container** - Default
      - **Size** - 0.1 GiB
      - Select **Add**

   - Select **Add New NIC**
      - **VLAN Name** - Primary

         .. note::

            Do **NOT** use your user configured VLAN. In CCA 2.1.X, the VM needs to exist in the same subnet as Prism Central. This issue is addressed in an upcoming release.

      - Select **Add**

#. Click **Save** to create the VM.

#. Select your VM and click **Actions > Power On**.

   .. note::

      By default, the CCA will try to acquire an IP address from a DHCP server. If you wish to set a static IP, use the console to access the CCA VM.

Configuring the CCA
+++++++++++++++++++

#. Note the **IP Address** of the *Initials*\ **-FrameCCA** VM in Prism, and open in the IP in a new browser tab to access the **Cloud Connector Configuration** wizard.

   .. figure:: images/3.png

#. Fill in the following fields and click **Log In** to connect the CCA to your Nutanix environment:

   - **Username** - Previously created *Initials*\ -FramceSvc account
   - **Password** - techX2020!
   - **Prism Central URL** - \https://<*Prism Central IP*>:9440

   .. figure:: images/4.png

#. Under **Select Cluster**, fill in the following fields and click **Next**:

   - **Cluster for virtual desktops** - *Your assigned cluster*
   - **Network for virtual desktops** - *Your assigned user network*
   - **Cloud account name** - *Initials*\ -\ *Cluster-Name*

   .. figure:: images/5.png

   .. note::

      You do not need to select **Enable enterprise profiles and personal drives** as this feature will not be used in the following exercises.

#. Under **Define Instance Types**, edit the existing profile name to **AHV 2vCPU 4GB** to better reflect the configuration. Add an additional custom **Instance Type**. Click **Next**.

   <Instance types are>

   .. figure:: images/6.png

#. Under **Select Sandbox Templates**, your *Initials*\ **-FrameImage** VM should automatically appear based on the **MasterTemplate** category value previously applied. Select the VM and specify **Windows 10** from the **OS** drop down. Click **Next**.

   .. figure:: images/7.png

#. The final step is to link your local infrastructure to the hosted Frame backplane. Under **Connect to Frame**, select **Sign in with My Nutanix** and provide your My Nutanix credentials if prompted. Once logged in, select the pre-created **nutanix.com Customer** and click **Finish**.

   .. figure:: images/8.png

   .. note::

      At this time, you cannot make any configuration changes to the Cloud Connector Appliance after it has been connected to the cluster. This functionality is coming soon. Please reach out to support@fra.me if you need to make any changes to your CCA.

#. Click **Go to Frame** to be redirected to the Xi Frame portal. Select **Organizations** from the left hand menu, and click :fa:`ellipsis-v` **> Cloud Accounts** to view the AHV Cloud Account creation status.

   .. figure:: images/9.png

   .. note::

      Click **Add Cloud Account** to see the wizard one would follow to add additional AWS, Azure, and GCP resources, all capable of being managed from the same Xi Frame portal.

   The **C** status indicates that the account is still being created. Prism Central will provision a Workload Proxy VM (**frame-workload-proxy-####**) in the desktop VLAN specified during CCA configuration. Once the status changes to **R**, indicating the workload proxy has been successfully provisioned, continue to the next exercise.

   .. figure:: images/10.png

WE DONE FAM
