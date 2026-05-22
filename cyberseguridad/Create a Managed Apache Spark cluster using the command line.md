- Create a Managed Apache Spark cluster using the command line.
- Run a simple Apache Spark job.
- Modify the number of workers in the cluster.
  
  We set the region
  ![[Pasted image 20260522182455.png]]
We disable de Cloud Dataproc API
![[Pasted image 20260522182554.png]]
We Re-enable Cloud Dataproc API
![[Pasted image 20260522182639.png]]
We run the following commands to grab the PROJECT_ID and PROJECT_NUMBER:
![[Pasted image 20260522182740.png]]
run the following command to give the Storage Admin and Managed Apache Spark Worker role to the Compute Engine default service account:
![[Pasted image 20260522182822.png]]
4. Run the following command to create a cluster called `example-cluster` with e2-standard-4 VMs and default Cloud Managed Apache Spark settings:
   ![[Pasted image 20260522182923.png]]
