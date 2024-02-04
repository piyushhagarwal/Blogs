---
title: "How are your passwords being stored?"
seoTitle: "How are your passwords being stored?"
seoDescription: "your passwords are stored in a variety of ways, some of which are more secure than others. It’s critical to understand how your passwords are stored and to"
datePublished: Wed Aug 02 2023 10:14:54 GMT+0000 (Coordinated Universal Time)
cuid: clktkndac000709jk2whtevbl
slug: how-are-your-passwords-being-stored
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690971115707/d0fa9a34-b78b-474b-ac82-b8765741e1a4.jpeg
tags: authentication, passwords

---

We all regularly sign up for a lot of websites; have you ever wondered how those websites store your passwords? Even so, is it safe?

> ## ***There are a total of four levels of authentication.***

The **first level** is the most basic and least secure; all that is required is to store the password exactly as it is in the database of a specific application. The passwords will be very easy for someone to access if they manage to hack the database.

![](https://miro.medium.com/v2/resize:fit:480/1*p7lQybKnjUQWPhHwg-zLXA.png align="center")

---

The password is encrypted and stored at the **second level**. The Caesar Cipher encryption method, which involves shifting each letter of the plaintext message by a specific number of letters, must be familiar to you. So in case, you want to decrypt the text or, in our case, the password, you should know the key, which is the number of letters shifted. For instance, the message “ABC” would be encoded as “def” if the key was 3.

![](https://miro.medium.com/v2/resize:fit:1200/1*VTWBF_xrygGGaRy2aL0E3g.png align="left")

This is a very simple illustration of encryption; today, messages or passwords can be encrypted using a variety of sophisticated methods. This level is also not particularly safe. If the key is revealed, the passwords can be easily decrypted. Therefore, as a developer, you must secure the key.

---

Let’s move on to the **third level**. This level is a significant improvement in terms of security. The password is hashed and stored in the database at this level.

### ***So, what exactly does hashing imply?***

Hashing is the process of passing a password(any string) to a hash function, which converts it to a unique hash code that is then saved in the database. MD5, SHA-1, SHA-2, NTLM, and LANMAN are some of the most commonly used hashing algorithms.

### *So, what’s the difference between steps 2 and 3 because both involve updating the actual password and putting it in the database?*

The difference is that encrypting the password is a two-way process, which means it is possible to decrypt the encrypted code using its key, whereas hashing is a one-way process, which means the password cannot be generated using the hash code. Finding the password from its hash code requires a significant amount of computational power. Consider the following example: suppose you were asked to find the prime factors of 377. It takes time; the factors are 13 and 27, but finding the multiplication of these two numbers would be much easier.

When a user attempts to log in, their password is hashed again, and this hash code is compared to the hash code already stored in the database; if they are equal, the user is logged in.

### *So, does this mean that no one can hack anyone’s password?*

The answer remains no; hackers have already built massive hash tables containing millions of hashed passwords. If the password’s hash code is revealed, the hacker will check whether or not its hash code is already present in these tables.

> *As a result, it is always advised to avoid using common, simple, or dictionary words as your password.*

![](https://miro.medium.com/v2/resize:fit:1400/1*0o2ehP0mJBGtBERD-qXRMw.png align="left")

---

The **fourth level**, the salt and pepper technique, is used to solve the above problem and make the password even more secure. Before hashing a password, random data called “salt” and a secret key called “pepper” are added.

The salt is a random string of characters that is generated for each password. It is then combined with the password and hashed. This ensures that even if two users have the same password, their salted and hashed passwords will be different.

The pepper, on the other hand, is a secret key that is known only to the system. It is added to the salted password before hashing it, making it even more difficult to crack.

By using both salt and pepper, the system makes it much harder for attackers to retrieve the original password from the hashed version. This is because the salt and pepper make it nearly impossible to use precomputed rainbow tables or other common password-cracking techniques.

![](https://miro.medium.com/v2/resize:fit:1400/1*AWJQLcqs1TgCAD-1yy7f_w.png align="left")

---

### Summary

In conclusion, your passwords are stored in a variety of ways, some of which are more secure than others. It’s critical to understand how your passwords are stored and to take precautions to keep your sensitive information safe. Always use strong, unique passwords, enable two-factor authentication, and avoid using the same password for multiple accounts. You can significantly reduce the risk of your passwords being compromised by implementing these best practices. Remember that putting in a little effort to secure your passwords can go a long way towards keeping your data safe and secure.

And as a developer, attempt to safeguard your users’ credentials as much as possible by using higher levels of authentication.

Thank you for reading. I appreciate you taking the time to read my post. Please feel free to share your thoughts in the comments below.