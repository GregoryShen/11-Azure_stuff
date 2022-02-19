# [Azure AD - Federated Domain vs. Managed Domain](https://blog.matrixpost.net/device-based-authentication-in-azure-ad-and-hybrid-mode-overview/)

When it comes to **Azure AD Authentication** in an **Hybrid environment**, where we had an on-premises and cloud environment, you can lose quickly the overview regarding the different options and terms for authentication in Azure AD.

We firstly need to distinguish between two fundamental different models to authenticate users in Azure and Office 365, there are **managed vs. federated domains** in **Azure AD**.

## Federated Domain

A **federated domain** means, that you have set up a federation between your on-premises environment and Azure AD. In this case **all user authentication is happen on-premises**. When a user logs into Azure or Office 365, their authentication request is forwarded to the on-premises AD FS server.

Because of the **federation trust** configured between both sites, **Azure AD** will trust the **security tokens** issued from the AD FS server at on-premises for authentication with Azure AD.

The **federation** itself is set up between your on-premises **Active Directory Federation Services (ADFS)** and **Azure AD** with the **Azure AD Connect** tool.

> *What is federation with Azure AD?*
>
> https://docs.microsoft.com/en-us/azure/active-directory/hybrid/whatis-fed
>
> Azure AD Connect and federation
>
> https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-fed-whatis

Further Azure supports Federation with **PingFederate** using the **Azure AD Connect** tool.

![](https://blog.matrixpost.net/wp-content/uploads/2021/05/auth_overview001.png)

## Managed Domain

A **managed domain** means, that you synchronize objects from your on-premises Active Directory to Azure AD, using the **Azure AD Connect** tool. Here you can choose between **Password Hash Synchronization** and **Pass-through authentication**.

When using **Password Hash Synchronization**, the authentication happens in **Azure AD** and with **Pass-through authentication**, the authentication still happens in **on-premises**.

> What is password hash synchronization with Azure AD?
>
> https://docs.microsoft.com/en-us/azure/active-directory/hybrid/whatis-phs
>
> Password hash synchronization is one of the sign-in methods used to accomplish hybrid identity. Azure AD Connect synchronizes a hash, of the hash, of a user's password from an on-premises Active Directory instance to a cloud-based Azure AD instance.
>
> What is Azure Active Directory Pass-through Authentication?
>
> https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-pta
>
> Azure Active Directory (Azure AD) Pass-through Authentication allows your users to sign in to both on-premises and cloud-based applications using the same passwords. When users sign in using Azure AD, this feature validates users' passwords directly against your on-premises Active Directory.

No matter if you use **federated** or **managed domains**, in all cases you can use the **Azure AD Connect** tool.

![](https://blog.matrixpost.net/wp-content/uploads/2021/05/auth_overview002.png)

## Single Sign-on to Azure and Office 365

