# GCP — Create a Managed Apache Spark cluster using the CLI

> Platform: Google Cloud · Category: Dataproc / Apache Spark

1. We set the region:

![](../_attachments/Pasted%20image%2020260527200935.png)

2. We disable the Cloud Dataproc API:

![](../_attachments/Pasted%20image%2020260527200958.png)

3. We re-enable the Cloud Dataproc API:

![](../_attachments/Pasted%20image%2020260527201041.png)

4. We run the following commands to grab the `PROJECT_ID` and `PROJECT_NUMBER`:

![](../_attachments/Pasted%20image%2020260527201148.png)

5. We run the following command to give the Storage Admin and Managed Apache Spark Worker roles to the Compute Engine default service account:

![](../_attachments/Pasted%20image%2020260527201228.png)

6. Run the following command to create a cluster called `example-cluster` with `e2-standard-4` VMs and default Cloud Managed Apache Spark settings:

![](../_attachments/Pasted%20image%2020260527201258.png)
