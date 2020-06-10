Demonstrating how would dangling release branch could cause issue to the changelog when done wrong


Develop has never been released, it's changelog looks like 

```
# Next

- Feature 2
- Feature 1
```

Someone prepared `release/0.1a` branch, modifies the changelog to following 

```
# 0.1

- Feature 2
- Feature 1
```

Someone finised `feature/3` and get that merged to `develop`

```
# Next

- Feature 3
- Feature 2
- Feature 1
```

When you try to merge `release/0.1a` to `develop` , problem arise, because it'd turn into

```
# 0.1

- Feature 3
- Feature 2
- Feature 1
```

You don't want it to happen because 0.1 does not contain feature 3

To fix this, we should always add the "# Next" tag in our release branch.
---

Checkout `release/0.1b`

```
# Next

# 0.1

- Feature 2
- Feature 1
```

When you try to merge `release/0.1b` into `develop`, it results in conflicts, which is what we wanted:

```
diff --cc CHANGELOG.md
index 2ac0fba,abfaef8..0000000
--- a/CHANGELOG.md
+++ b/CHANGELOG.md
@@@ -1,6 -1,7 +1,11 @@@
  # Next

++<<<<<<< HEAD
 +- Feature 3
++=======
+ # 0.1
+
++>>>>>>> release/0.1b
  - Feature 2
  - Feature 1
```

We could see "- Feature 3" will be conflicting with the "# 0.1" tag, and then you know you'll need to keep the feature in the "Next" section.

