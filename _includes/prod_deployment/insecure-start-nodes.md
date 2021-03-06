For each initial node of your cluster, complete the following steps:

{{site.data.alerts.callout_info}}After completing these steps, nodes will not yet be live. They will complete the startup process and join together to form a cluster as soon as the cluster is initialized in the next step.{{site.data.alerts.end}}

1. SSH to the machine where you want the node to run.

2. Download the [CockroachDB archive](https://binaries.cockroachdb.com/cockroach-{{ page.release_info.version }}.linux-amd64.tgz) for Linux, and extract the binary:

    {% include copy-clipboard.html %}
    ~~~ shell
    $ wget -qO- https://binaries.cockroachdb.com/cockroach-{{ page.release_info.version }}.linux-amd64.tgz \
    | tar  xvz
    ~~~

3. Copy the binary into the `PATH`:

    {% include copy-clipboard.html %}
    ~~~ shell
    $ cp -i cockroach-{{ page.release_info.version }}.linux-amd64/cockroach /usr/local/bin
    ~~~

    If you get a permissions error, prefix the command with `sudo`.

4. Run the [`cockroach start`](start-a-node.html) command:

    {% include copy-clipboard.html %}
    ~~~
    $ cockroach start --insecure \
    --host=<node1 address> \
    --join=<node1 address>:26257,<node2 address>:26257,<node3 address>:26257 \
    --cache=25% \
    --max-sql-memory=25% \
    --background
    ~~~

    This command primes the node to start, using the following flags:

    Flag | Description
    -----|------------
    `--insecure` | Indicates that the cluster is insecure, with no network encryption or authentication.
    `--host` | Specifies the hostname or IP address to listen on for intra-cluster and client communication. If it is a hostname, it must be resolvable from all nodes, and if it is an IP address, it must be routable from all nodes.<br><br>If you want to listen on multiple interfaces, leave `--host` empty. If doing
    `--join` | Identifies the address and port of 3-5 of the initial nodes of the cluster.
    `--cache`<br>`--max-sql-memory` | Increases the node's cache and temporary SQL memory size to 25% of available system memory to improve read performance and increase capacity for in-memory SQL processing (see [Recommended Production Settings](recommended-production-settings.html#cache-and-sql-memory-size-changed-in-v1-1) for more details).
    `background` | Starts the node in the background so you gain control of the terminal to issue more commands.

	For other flags not explicitly set, the command uses default values. For example, the node stores data in `--store=cockroach-data`, binds internal and client communication to `--port=26257`, and binds Admin UI HTTP requests to `--http-port=8080`. To set these options manually, see [Start a Node](start-a-node.html).

5. Repeat these steps for each addition node that you want in your cluster.
