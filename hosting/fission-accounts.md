# Fission Accounts

When you create a Fission Account, whether using the CLI, or signing up on the web, it creates a username and email address in our service database, and also a private / public key pair representing that account.

We also create a Fission File System attached to your account, and given you access to[ Fission Drive](https://guide.fission.codes/drive), which lets you browse all your files, access them from any browser, and see which apps are attached to your File System.

So far, no passwords! We verify your email address, and can use that to help manage aspects of your service with us. You hold access to the keys connected to your account, and sign in happens automatically.

We encourage you to "link" your account to multiple devices -- your desktop and your phone, your home and your work computers, and so on. These devices can then be used to login, or link more accounts. You can read more about [Account Linking »](account-linking.md)

## Signing out of Fission Apps

There is no "sign out" for a Fission-powered app. You use your key to do a passwordless login, stored in your local desktop browser, mobile web browser, or your local desktop file system with the command line developer tool.

While your device is linked with a key, Fission apps will check that you have a key with the proper permission, and will just let you right in, without having to remember a password or even a username.

This is like your smartphone: only a single user is "logged in" to their personal phone, and they aren't shared.

{% hint style="info" %}
A Fission app is more like an app that you download on your phone. When you don't want the app anymore, you delete it. 

If you want it again in the future, you download it again, by giving permission to the app store to install it. You then might give permission to access other parts of your phone, like the camera or GPS.
{% endhint %}

We don't delete the data that the app stored for you, since it's stored in your own Fission File System -- just like data is stored on your phone.

## Revoking App Access

Instead of signing out, you may want to revoke -- or delete -- an app's access to your account. You can visit the Fission Auth page to see both devices and apps that are connected to your account.

{% hint style="warning" %}
You can even revoke access to the default Drive app, but we'll ask you to make an extra confirmation. You'll need to use another tool or developer API access to manage your Fission File System attached to your account.
{% endhint %}

## Shared Devices

But, browsers and desktop computers aren't smartphones, and they do get shared. You can unlink a device -- remove your key -- by visiting the Fission Auth page.

## Multiple Accounts per Device

You may create multiple Fission accounts, but you'll need a unique email address and username for each one. You'll also need to use Browser Profiles to be able to access them at the same time on the same machine, as the keys that grant access are stored in the browser.



