# TiDB on TiCat: Why it is Needed

## The issue
The TiDB system include many dimensions:
* Cluster hardwares and topologies
* Cluster maintenances: deploy, stop, start, scale in/out
* Data migrating
* Runtime status display/scanning/analyzing tools
* All MySQL compatible workloads
* ...

It's hard to cover all casts since the possible collection set is huge from these dimensions multiply together.

TiDB developing uses an automatic centerized-integrating procedure to ensure the production quality and performance,
that could only cover some typical usages, far from a decent benchmark.

This compromised both costs and qualities in TiDB developing.

## The cure
By using [ticat](https://github.com/innerr/ticat), any utilities/tools could easily wrap into components(aka, modules).
All components seamlessly work togather, form a full-featured system.

To maximize the ability and efficiency of TiDB non-production activities,
We adopt the most aggressive approach: ad-hot feature assembling.

That means a developer could select the necessary components based on on-site purpose,
concate them into a workflow (like unix-pipe), then get the job done (faster and better).

Even more, the assembled workflows could be easily shared to others,
so anyone could benefit from this, even they know nothing of [ticat](https://github.com/innerr/ticat).

By breaking centerized-integration into perpendicular components,
we have the potential to cover all casts.
By the end-user assembling, we get a powerful yet flexable system.


# Build a better TiDB ecosystem with ticat

## The problem: lonely and ineffective fighters
Building a big, rich-featured system is difficult,
to control the quality normally we have hard boundaries of the system,
all things **in** it are expected to be well designed and tested.

Take "auto deployment" as an example,
anyone proposal this will encounter lots of questions from PMG (product manage group):
* Any product level cluster should be carefully check about the topology, why we need it?
* What hardwares should we support?
* What OS should we support
* ...
Even this feature could pass through PMG and come to online,
All those questions still there and become oncall-issues.

Hence we don't have the ability to have "auto deployment" inside the boundaries,
it costs too much.

But as TiDB developers we deploy clusters a lot,
smart guys write "quick and dirty" scripts to solve that. Or not, that's worse.

This is the problem we have, inside the core-system everything is (fairly) tidy and clean,
outside of it things are out of control, each developer/user fights alone.

## Rock with ticat
With **ticat** a workflow is a pipeline like `<get hardware resource> : <deploy TiDB> : <run tpcc>`,
we don't need a well implemented "auto deploy" tool,
it could be the "quick and dirty" scripts wrapped as a component,
typically could be a yaml config template for 1PD-1TiDB-3TiKV deployment.

The ad-hot assembling let end-user to select the suitable deployer,
user get it from others by adding git repo to **ticat**,
if it works fine that's great, if not, user could improve it by forking the repo,
then re-share the repo address.

Here is something great with **ticat**:
* No publish center, anyone could be a publish center, code could be shared and execute without approvement from authority.
* Significantly lower the feature requirement, a component only need to solve things in specific cases.
* Smooth enveloping, a component can be "dirty" at first, could be insteaded by better one from other git repo.

Running TiDB with **ticat** soften the community boundaries,
the "outside" we used to fight alone now become gray area we could share "dirty" scripts,
and grow features can be put into core-system.

Through that we achieve fully automation, reduce costs, enhance developing env,
thus make a better product and a better ecosystem.
