---
sidebar_label: 'Managing quotas per project'
sidebar_position: 25
---

# Managing quotas per project

For each project, it is possible to configure quota limits to control the project expenses or usage. 

To access and configure quota limits, you need the Organization role.

In the group of options called **Organization Options**, you will find the **Manage Projects** section, where you can manage quota 
limits for each project.

To do this, select the desired project and click on the USAGE LIMITS link:

![image](https://github.com/genexus-books/Saia/blob/b6bf1de2bdc955b955a602b7b21d292c127e3adc/saia-docs/assets/images/gx-eai-UsageLimits.png?raw=true)

## Limit definition

It is possible to define quota limits based on:

*	Membership: Freemium(), daily, weekly, or monthly.
* Unit: Cost(USD) or request.

![image](https://github.com/genexus-books/Saia/blob/b6bf1de2bdc955b955a602b7b21d292c127e3adc/saia-docs/assets/images/gx-eai-MembershipUnit.png?raw=true)

When a Membership is assigned a Freemium value, it means that the membership allows a one-time use, a free trial or a specific
limit of requests or costs, without an expiration date.

Furthermore, you can define a **Soft limit alert** and a **Hard limit** for each quota limit.  

![image](https://github.com/genexus-books/Saia/blob/b6bf1de2bdc955b955a602b7b21d292c127e3adc/saia-docs/assets/images/gx-eai-SoftHardLimit.png?raw=true)

An e-mail alert will be sent to the project manager when the defined value in **Soft limit alert** is reached.

In addition, when the value defined in **Hard limit** is reached, the platform does not allow executing the request and logs 
the error.

Finally, the **Renewable?** option allows you to indicate if the **Quota Limit** is renewable, as long as the Membership value is 
different from Freemium.

## Quota limit status

A quota limit can have three statuses: Active, Expired, or Empty. When the set **Hard limit** is reached, the quota status is 
changed to Empty.

For projects with active quotas, it is not possible to define new quotas. In this situation, it is necessary to edit the 
active quota and change its status to Expired. 

In addition, you will be able to see the amount of available quota and the amount used:

![image](https://github.com/genexus-books/Saia/blob/b6bf1de2bdc955b955a602b7b21d292c127e3adc/saia-docs/assets/images/gx-eai-UsedRemaining.png?raw=true)
