# beego-authz [![Coverage Status](https://coveralls.io/repos/github/casbin/beego-authz/badge.svg?branch=master)](https://coveralls.io/github/casbin/beego-authz?branch=master) [![GoDoc](https://godoc.org/github.com/casbin/beego-authz?status.svg)](https://godoc.org/github.com/casbin/beego-authz)

``beego-authz`` is an authorization middleware for [Beego](https://beego.me/). It provides authorization like ACL, RBAC, ABAC based on Casbin: https://github.com/casbin/casbin

With ``beego-authz``, you can control who can access what resource via which method for your Beego app.

## Get Started

### Step 1: edit the policy

Modify the Casbin model: [authz_model.conf](https://github.com/casbin/beego-authz/blob/master/authz/authz_model.conf) and policy: [authz_policy.csv](https://github.com/casbin/beego-authz/blob/master/authz/authz_policy.csv) as you want. You may need to learn Casbin's basics to know how to edit these files. The policy means that the user ``alice`` can access ``/dataset1/*`` via ``GET`` and ``/dataset1/resource1`` via ``POST``. The similar way applies to user ``bob``. ``cathy`` has the role ``dataset1_admin``, which is permitted to access any resources under ``/dataset1/`` with any action. For more advanced usage of Casbin (like database support, policy language grammar, etc), please refer to Casbin: https://github.com/casbin/casbin

### Step 2: integrate with Beego

Insert the [Casbin authorizer](https://github.com/casbin/beego-authz/blob/master/authz/authz.go) as a Beego filter.

```go
beego.InsertFilter("*", beego.BeforeRouter, authz.NewAuthorizer(casbin.NewEnforcer("authz_model.conf", "authz_policy.csv")))
```

### Step 3: setup with authentication

Make sure you already have an authentication mechanism, so you know who is accessing, like a username. Modify the [GetUserName()](https://github.com/casbin/beego-authz/blob/master/authz/authz.go#L68-L71) method to let Casbin know the current authenticated username.

## Getting Help

- [Casbin](https://github.com/casbin/casbin)

## License

This project is under Apache 2.0 License. See the [LICENSE](https://github.com/casbin/beego-authz/blob/master/LICENSE) file for the full license text.
