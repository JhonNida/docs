---
title: 使用组管理自托管运行器的访问权限
intro: 您可以使用策略来限制对已添加到组织或企业的自托管运行器的访问。
redirect_from:
  - /actions/hosting-your-own-runners/managing-access-to-self-hosted-runners
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
type: tutorial
shortTitle: 管理运行器组
---

{% data reusables.actions.ae-self-hosted-runners-notice %}
{% data reusables.actions.enterprise-beta %}
{% data reusables.actions.enterprise-github-hosted-runners %}

## 关于自托管运行器组

{% ifversion fpt %}
{% note %}

**注：**所有组织都有一个默认的自托管运行器组。 只有企业帐户和企业帐户拥有的组织才能创建和管理其他自托管的运行器组。

{% endnote %}

Self-hosted runner groups are used to control access to self-hosted runners. 组织管理员可以配置访问策略，用以控制组织中的哪些组织可以访问运行器组。
如果您使用

{% data variables.product.prodname_ghe_cloud %}, you can create additional runner groups; enterprise admins can configure access policies that control which organizations in an enterprise have access to the runner group; and organization admins can assign additional granular repository access policies to the enterprise runner group. For more information, see the [{% data variables.product.prodname_ghe_cloud %} documentation](/enterprise-cloud@latest/actions/hosting-your-own-runners/managing-access-to-self-hosted-runners-using-groups).
{% endif %}

{% ifversion ghec or ghes or ghae %}
自托管运行器组用于控制对组织和企业级自托管运行器的访问。 企业管理员可以配置访问策略，用以控制企业中的哪些组织可以访问运行器组。 组织管理员可以配置访问策略，用以控制组织中的哪些组织可以访问运行器组。

当企业管理员授予组织对运行器组的访问权限时，组织管理员可以看到组织的自托管运行器设置中列出的运行器组。 然后，组织管理员可以为企业运行器组分配其他细致的仓库访问策略。

新运行器在创建时，将自动分配给默认组。 运行器每次只能在一个组中。 您可以将运行器从默认组移到另一组。 更多信息请参阅“[将自托管运行器移动到组](#moving-a-self-hosted-runner-to-a-group)”。

## 为组织创建自托管的运行器组

所有组织都有一个默认的自托管运行器组。 企业帐户中的组织可以创建其他自托管组。 组织管理员可以允许单个仓库访问运行器组。 有关如何使用 REST API 创建自托管运行器组的信息，请参阅“[自托管运行器组](/rest/reference/actions#self-hosted-runner-groups)”。

自托管运行器在创建时会自动分配给默认组，并且每次只能成为一个组的成员。 您可以将运行器从默认组移到您创建的任何组。

创建组时，必须选择用于定义哪些仓库有权访问运行器组的策略。

{% ifversion ghec or ghes > 3.3 or ghae-issue-5091 %}
{% data reusables.organizations.navigate-to-org %}
{% data reusables.organizations.org_settings %}
{% data reusables.github-actions.settings-sidebar-actions-runner-groups %}
1. 在“Runner groups（运行器组）”部分，单击 **New runner group（新运行器组）**。
 {% data reusables.github-actions.runner-group-assign-policy-repo %}

   {% warning %}

   **警告：** {% indented_data_reference reusables.github-actions.self-hosted-runner-security spaces=3 %}

   更多信息请参阅“[关于自托管运行器](/actions/hosting-your-own-runners/about-self-hosted-runners#self-hosted-runner-security-with-public-repositories)”。

   {% endwarning %}
{% data reusables.github-actions.self-hosted-runner-create-group %}
{% elsif ghae or ghes < 3.4 %}
{% data reusables.organizations.navigate-to-org %}
{% data reusables.organizations.org_settings %}
{% data reusables.github-actions.settings-sidebar-actions-runners %}
1. Under {% ifversion ghes > 3.1 or ghae %}"Runners"{% elsif ghes < 3.2 %}"Self-hosted runners"{% endif %}, click **Add new**, and then **New group**.

    ![添加运行器组](/assets/images/help/settings/actions-org-add-runner-group.png)
1. 输入运行程序组的名称，并分配仓库访问策略。

   {% ifversion ghes or ghae %} 您可以配置一个运行器组可供一组特定的仓库或组织中所有仓库访问。 默认情况下，只有私有仓库可以访问运行器组中的运行器，但您可以覆盖此设置。 This setting can't be overridden if configuring an organization's runner group that was shared by an enterprise.{% endif %}

   {% warning %}

   **警告**

   {% indented_data_reference reusables.github-actions.self-hosted-runner-security spaces=3 %}

   更多信息请参阅“[关于自托管运行器](/actions/hosting-your-own-runners/about-self-hosted-runners#self-hosted-runner-security-with-public-repositories)”。

   {% endwarning %}

   ![添加运行器组选项](/assets/images/help/settings/actions-org-add-runner-group-options.png)
1. 单击 **Save group（保存组）**创建组并应用策略。
{% endif %}

## 为企业创建自托管运行器组

企业可以将其自托管的运行器添加到组以进行访问管理。 企业可以创建供企业帐户中特定组织访问的自托管运行器组。 然后，组织管理员可以为企业运行器组分配其他细致的仓库访问策略。 有关如何使用 REST API 创建自托管运行器组的信息，请参阅[企业管理 GitHub Actions API](/rest/reference/enterprise-admin#github-actions)。

自托管运行器在创建时会自动分配给默认组，并且每次只能成为一个组的成员。 您可以在注册过程中将运行器分配给特定组，也可以稍后将运行器从默认组移到自定义组。

创建组时，必须选择用于定义哪些组织有权访问运行器组的策略。

{% ifversion ghec or ghes > 3.3 or ghae-issue-5091 %}
{% data reusables.enterprise-accounts.access-enterprise %}
{% data reusables.enterprise-accounts.policies-tab %}
{% data reusables.enterprise-accounts.actions-tab %}
{% data reusables.enterprise-accounts.actions-runner-groups-tab %}
1. 单击 **New runner group（新运行器组）**。
 {% data reusables.github-actions.runner-group-assign-policy-org %}

   {% warning %}

   **警告**

   {% indented_data_reference reusables.github-actions.self-hosted-runner-security spaces=3 %}

   更多信息请参阅“[关于自托管运行器](/actions/hosting-your-own-runners/about-self-hosted-runners#self-hosted-runner-security-with-public-repositories)”。

   {% endwarning %}
{% data reusables.github-actions.self-hosted-runner-create-group %}
{% elsif ghae or ghes < 3.4 %}
{% data reusables.enterprise-accounts.access-enterprise %}
{% data reusables.enterprise-accounts.policies-tab %}
{% data reusables.enterprise-accounts.actions-tab %}
{% data reusables.enterprise-accounts.actions-runners-tab %}
1. 单击 **Add new（新增）**，然后单击 **New group（新组）**。

    ![添加运行器组](/assets/images/help/settings/actions-enterprise-account-add-runner-group.png)
1. 输入运行程序组的名称，并分配组织访问策略。

    您可以配置运行器组供特定的组织列表或企业中所有组织访问。 默认情况下，只有私有仓库可以访问运行器组中的运行器，但您可以覆盖此设置。 This setting can't be overridden if configuring an organization's runner group that was shared by an enterprise.

   {% warning %}

   **警告**

   {% indented_data_reference reusables.github-actions.self-hosted-runner-security spaces=3 %}

   更多信息请参阅“[关于自托管运行器](/actions/hosting-your-own-runners/about-self-hosted-runners#self-hosted-runner-security-with-public-repositories)”。

   {% endwarning %}

    ![添加运行器组选项](/assets/images/help/settings/actions-enterprise-account-add-runner-group-options.png)
1. 单击 **Save group（保存组）**创建组并应用策略。
{% endif %}
{% endif %}

## 更改自托管运行器组的访问策略

您可以更新运行器组的访问策略，或重命名运行器组。
{% ifversion fpt or ghec or ghes > 3.3 or ghae-issue-5091 %}
{% data reusables.github-actions.self-hosted-runner-groups-navigate-to-repo-org-enterprise %}
{% data reusables.github-actions.settings-sidebar-actions-runner-groups-selection %}
1. 修改访问选项或更改运行器组名称。

   {% warning %}

   **警告**

   {% indented_data_reference reusables.github-actions.self-hosted-runner-security spaces=3 %}

   更多信息请参阅“[关于自托管运行器](/actions/hosting-your-own-runners/about-self-hosted-runners#self-hosted-runner-security-with-public-repositories)”。

   {% endwarning %}
{% elsif ghae or ghes < 3.4 %}
{% data reusables.github-actions.self-hosted-runner-configure-runner-group-access %}
{% endif %}

{% ifversion ghec or ghes or ghae %}
## 自动向组添加自托管运行器

您可以使用配置脚本自动向组添加新的自托管运行器。 例如， 此命令将注册一个新的自托管运行器，并使用 `--runnergroup` 参数将其添加到名为 `rg-runnergroup` 的组。

```sh
./config.sh --url $org_or_enterprise_url --token $token --runnergroup rg-runnergroup
```

如果运行器组不存在，命令将失败：

```
找不到名为 "rg-runnergroup" 的任何自托管运行器组。
```

## 将自托管的运行器移动到组

如果您在注册过程中没有指定运行器组，新的自托管运行器将自动分配到默认组，然后可以移到另一个组。

{% data reusables.github-actions.self-hosted-runner-navigate-to-org-enterprise %}
{% ifversion ghec or ghes > 3.3 or ghae-issue-5091 %}
1. 在“Runners（运行器）”列表中，单击您要配置的运行器。
2. 选择运行器组下拉菜单。
3. 在“Move runner to group（将运行器移动到组）”中，选择运行器的目的地组。
{% elsif ghae or ghes < 3.4 %}
1. In the {% ifversion ghes > 3.1 or ghae %}"Runner groups"{% elsif ghes < 3.2 %}"Self-hosted runners"{% endif %} section of the settings page, locate the current group of the runner you want to move and expand the list of group members. ![查看运行器组成员](/assets/images/help/settings/actions-org-runner-group-members.png)
2. 选中自托管运行器旁边的复选框，然后单击 **Move to group（移动到组）**以查看可用的目的地。 ![运行器组成员移动](/assets/images/help/settings/actions-org-runner-group-member-move.png)
3. 要移动运行器，请单击目标组。 ![运行器组成员移动](/assets/images/help/settings/actions-org-runner-group-member-move-destination.png)
{% endif %}

## 删除自托管运行器组

自托管运行器在其组被删除时将自动返回到默认组。

{% ifversion ghes > 3.1 or ghae or ghec %}
{% data reusables.github-actions.self-hosted-runner-groups-navigate-to-repo-org-enterprise %}
1. 在组列表中，在要删除的组右侧，单击 {% octicon "kebab-horizontal" aria-label="The horizontal kebab icon" %}。
2. 要删除组，请单击 **Remove group（删除组）**。
3. 查看确认提示，然后单击 **Remove this runner group（删除此运行器组）**。
{% elsif ghes < 3.2 %}
1. 在设置页面的“Self-hosted runners（自托管运行器）”部分，找到要删除的组，然后单击 {% octicon "kebab-horizontal" aria-label="The horizontal kebab icon" %} 按钮。 ![查看运行器组设置](/assets/images/help/settings/actions-org-runner-group-kebab.png)

1. 要删除组，请单击 **Remove group（删除组）**。 ![查看运行器组设置](/assets/images/help/settings/actions-org-runner-group-remove.png)

1. 查看确认提示，然后单击 **Remove this runner group（删除此运行器组）**。
{% endif %}
{% endif %}
