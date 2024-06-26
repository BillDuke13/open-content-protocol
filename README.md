# open-content-protocol

[繁體中文](./docs/README_zh-TW.md) | [Français](./docs/README_fr.md) | [한국어](./docs/README_ko.md)

## Overview

This is a content platform based on the Sui blockchain, aimed at providing a decentralized, censorship-resistant content value network for content providers and individual creators.

## Architecture

```mermaid
graph TB;
    %% Define the styles directly within the graph
    style Platform fill:#ffffff,stroke:#000000,stroke-width:2px;
    style CreatorObj fill:#ffffff,stroke:#000000,stroke-width:2px;
    style PostKeyObj fill:#ffffff,stroke:#000000,stroke-width:2px;
    style ContentCreator fill:#ffffff,stroke:#000000,stroke-width:2px;
    style PostObj fill:#ffffff,stroke:#000000,stroke-width:2px;
    style PaidObj fill:#ffffff,stroke:#000000,stroke-width:2px;
    style MemberObj fill:#ffffff,stroke:#000000,stroke-width:2px;
    style SubscriberObj fill:#ffffff,stroke:#000000,stroke-width:2px;
    style MemberDetails fill:#ffffff,stroke:#000000,stroke-width:2px;
    style SubscriberDetails fill:#ffffff,stroke:#000000,stroke-width:2px;

    subgraph PlatformManagement["Platform Management"]
        Platform["Platform"]
        CreatorObj["Creator Object"]
        PostKeyObj["PostKey Object"]
    end

    subgraph ContentCreation["Content Creation"]
        ContentCreator["Content Creator"]
        PostObj["Post Object"]
        PaidObj["Paid(Kiosk) Object"]
    end

    subgraph UserRoles["User Roles"]
        MemberObj["Member Object"]
        SubscriberObj["Subscriber Object"]
        MemberDetails["Details (url, desc, expiry)"]
        SubscriberDetails["Details (url, desc, no expiry)"]
    end

    Platform -->|Manages| CreatorObj
    Platform -->|Manages| PostKeyObj
    PostKeyObj -->|Unlocks| PostObj

    CreatorObj -->|Creates Content| ContentCreator
    ContentCreator -->|Posts| PostObj
    ContentCreator -->|Creates Paid| PaidObj

    PaidObj -->|Paid| MemberObj

    MemberObj -->|Details| MemberDetails
    SubscriberObj -->|Details| SubscriberDetails

    PostObj -.->|Access| MemberObj
    PostObj -.->|Access| SubscriberObj
```

## Smart Contract Functions

### Create a Creator

` sui client call --package <package_id> --module ocp_creator --function mint_creator --args <url> <description> <avatar> <member_prices> --gas-budget <gas_budget> `

### Update Creator Information

` sui client call --package <package_id> --module ocp_creator --function update_creator --args <creator_id> <url> <description> <avatar> --gas-budget <gas_budget> `

### Create a Post

` sui client call --package <package_id> --module ocp_creator --function mint_post --args <creator_id> <url> <description> <access_level> --gas-budget <gas_budget> `

### Update Post Information

` sui client call --package <package_id> --module ocp_creator --function update_post --args <post_id> <url> <description> <access_level> --gas-budget <gas_budget> `

### Create a Post Access Key (PostKey)

` sui client call --package <package_id> --module ocp_creator --function mint_post_key --args <post_id> <access_level> <owner> --gas-budget <gas_budget> `

### Create a Paid

` sui client call --package <package_id> --module ocp_paid --function mint_paid --args <creator_id> <url> <description> --gas-budget <gas_budget> `

### Request Custom Content(Kiosk)

` sui client call --package <package_id> --module ocp_paid --function request_custom_paid --args <creator> <description> <payment> --gas-budget <gas_budget> `


### Deliver Custom Content(Kiosk)

` sui client call --package <package_id> --module ocp_paid --function fulfill_custom_request --args <kiosk_id> <request_id> <url> --gas-budget <gas_budget> `

### Create a Membership (Member)

` sui client call --package <package_id> --module ocp_member --function mint_member --args <creator> <url> <description> <avatar> <clock_id> --gas-budget <gas_budget> `

### Renew Membership

` sui client call --package <package_id> --module ocp_member --function renew_member --args <member_id> <creator_id> <price_index> <payment> <clock_id> --gas-budget <gas_budget> `

### Create a Subscriber

` sui client call --package <package_id> --module ocp_subscriber --function mint_subscriber --args <creator> <url> <description> <avatar> --gas-budget <gas_budget> `

### Update Subscriber Information

` sui client call --package <package_id> --module ocp_subscriber --function update_subscriber --args <subscriber_id> <url> <description> <avatar> --gas-budget <gas_budget> `

## License

This project is licensed under the Apache 2.0 License. For more details, please refer to the [LICENSE](./LICENSE) file.