# Security

## Services security

The forbidden intersection is:

-   Running as root, or with `CAP_SYS_ADMIN`, _and_
-   Running in the init PID and mount namespaces, _and_
-   Running without a Seccomp policy or without an enforcing SELinux domain.

You **must** avoid the forbidden intersection by having at least one of, preferably more than one of, and ideally all of:

-   Running as a non-root user with no `CAP_SYS_ADMIN` — [User IDs](https://chromium.googlesource.com/chromiumos/docs/+/HEAD/sandboxing.md#User-IDs)
-   Running in a non-init mount and PID namespace — [Namespaces](https://chromium.googlesource.com/chromiumos/docs/+/HEAD/sandboxing.md#Namespaces)
-   A Seccomp policy OR running with an SELinux policy in an enforcing domain — [Seccomp filters](https://chromium.googlesource.com/chromiumos/docs/+/HEAD/sandboxing.md#Seccomp-filters) or [SELinux](https://chromium.googlesource.com/chromiumos/docs/+/HEAD/security/selinux.md)

You don't normally need both Seccomp and SELinux but for very security-sensitive workloads this can be required.

-   Create a new user for your service ([example](https://crrev.com/c/225257)). Use [this README](https://chromium.googlesource.com/chromiumos/overlays/eclass-overlay/+/HEAD/profiles/base/accounts/README.md) as a guide on choosing and testing the new UID and GID.
-   Optionally, create a new group to control access to a resource and add the new user to that group ([example](https://crrev.com/c/242551)).
-   Use Minijail to run your service as the user (and group) created in the previous steps. In your init script:
    -   `exec minijail0 -u <user> -g <group> /full/path/to/binary`
    -   See [User IDs](https://chromium.googlesource.com/chromiumos/docs/+/HEAD/sandboxing.md#User-IDs).
-   If your service fails, you might need to grant it capabilities. See [Capabilities](https://chromium.googlesource.com/chromiumos/docs/+/HEAD/sandboxing.md#Capabilities).
-   Use as many namespaces as possible. See [Namespaces](https://chromium.googlesource.com/chromiumos/docs/+/HEAD/sandboxing.md#Namespaces).
-   Consider reducing the kernel attack surface exposed to your service by using Seccomp filters. See [Seccomp filters](https://chromium.googlesource.com/chromiumos/docs/+/HEAD/sandboxing.md#Seccomp-filters).
-   Add your sandboxed service to the [security.SandboxedServices](https://chromium.googlesource.com/chromiumos/platform/tast-tests/+/HEAD/src/chromiumos/tast/local/bundles/cros/security/sandboxed_services.go) test.