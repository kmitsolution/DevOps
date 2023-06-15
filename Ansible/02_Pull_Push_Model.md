## Pull Based Model
The pull-based configuration model in DevOps refers to a configuration management approach where target systems actively retrieve their configuration from a central source. In this model, the configuration management server or repository acts as the source of truth for configurations, and the target systems periodically or on-demand pull the latest configuration changes.
<img src="https://github.com/kmitsolution/DevOps/blob/main/Ansible/images/pullbased.JPG" width=900 height=400 />

Here are the key characteristics and benefits of the pull-based configuration model:

1. <b>Centralized configuration:</b> The configuration management server holds the authoritative copy of the configurations for the target systems. It serves as a central source of truth, ensuring consistency across the infrastructure.
2. <b>Periodic or on-demand updates:</b> The target systems periodically or manually request configuration updates from the configuration management server. This allows for flexibility in updating configurations based on predefined schedules or triggered by specific events.
3. <b>Declarative nature:</b> Configuration management tools typically follow a declarative approach, where the desired state of the target systems is defined in configuration files or manifests. The tools handle the retrieval and enforcement of the configurations, ensuring the systems converge to the desired state.
4. <b>Scalability:</b> The pull-based model scales well, as the target systems independently fetch their configurations from the centralized server. This allows for efficient management of large-scale infrastructures with numerous target systems.
5. <b>Reduced impact on target systems:</b> In the pull-based model, target systems only retrieve configuration updates when needed. This reduces the impact on the systems and network, as they are not constantly being pushed with updates.
### Examples:
 In Puppet, the client pulls configurations from the server.

## Push Based Model
The push-based configuration model in DevOps refers to a configuration management approach where the configuration changes are actively pushed from a central source to the target systems. In this model, the configuration management server initiates the distribution of configurations and ensures they are applied to the target systems.
<img src="https://github.com/kmitsolution/DevOps/blob/main/Ansible/images/Pushbased.JPG" width=800 height=400 />

Here are the key characteristics and benefits of the push-based configuration model:

1. <b>Centralized control:</b> In the push-based model, the configuration management server initiates and manages the distribution of configurations to the target systems. It provides a centralized point of control, allowing administrators to enforce configurations uniformly across the infrastructure.
2. <b>Real-time or scheduled updates:</b> The configuration management server pushes the configurations to the target systems either immediately upon changes or according to a predefined schedule. This allows for timely and consistent application of configurations across the infrastructure.
3. <b>Imperative nature:</b> Configuration management tools or scripts explicitly define the steps and actions needed to push the configurations to the target systems. This provides fine-grained control over the configuration application process.
4. <b>Quick reaction to configuration changes:</b> With the push-based model, configuration changes can be propagated to the target systems in real-time or near real-time. This enables quick reactions to configuration updates, ensuring that systems are always up to date.

### Examples: 
Tools like Ansible,SaltStack are commonly associated with the push-based model. These tools utilize agents or agentless communication to push the desired configurations to the target systems.

The push-based configuration model provides benefits such as centralized control, real-time updates, and the ability to react quickly to configuration changes. It allows administrators to maintain strict control over the configuration state of the infrastructure and enforce configurations uniformly.

However, it's important to consider factors such as network bandwidth, security implications, and potential disruptions when implementing the push-based model, especially in large-scale and highly dynamic environments. Regular communication between the configuration management server and the target systems is crucial to ensure timely and accurate configuration updates.

