---
title: Sobre a autorização em aplicativos Web e em microsserviços .NET
description: Segurança nos microsserviços e aplicativos Web do .NET – obtenha uma visão geral das principais opções de autorização em aplicativos ASP.NET Core (baseadas em função e baseadas em políticas).
author: mjrousos
ms.author: wiwagn
ms.date: 10/19/2018
ms.openlocfilehash: 36cd8eaf7ffe78a29762398044dc1803adc1b200
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68675653"
---
# <a name="about-authorization-in-net-microservices-and-web-applications"></a><span data-ttu-id="76d4d-103">Sobre a autorização em aplicativos Web e em microsserviços .NET</span><span class="sxs-lookup"><span data-stu-id="76d4d-103">About authorization in .NET microservices and web applications</span></span>

<span data-ttu-id="76d4d-104">Após a autenticação, as APIs Web do ASP.NET Core precisam autorizar o acesso.</span><span class="sxs-lookup"><span data-stu-id="76d4d-104">After authentication, ASP.NET Core Web APIs need to authorize access.</span></span> <span data-ttu-id="76d4d-105">Esse processo permite que um serviço disponibilize APIs para alguns usuários autenticados, mas não para todos.</span><span class="sxs-lookup"><span data-stu-id="76d4d-105">This process allows a service to make APIs available to some authenticated users, but not to all.</span></span> <span data-ttu-id="76d4d-106">A [autorização](/aspnet/core/security/authorization/introduction) pode ser dada com base em funções de usuários ou em política personalizada, que pode incluir inspeção de declarações ou outros fatores.</span><span class="sxs-lookup"><span data-stu-id="76d4d-106">[Authorization](/aspnet/core/security/authorization/introduction) can be done based on users’ roles or based on custom policy, which might include inspecting claims or other heuristics.</span></span>

<span data-ttu-id="76d4d-107">Restringir o acesso a uma rota do ASP.NET Core MVC é tão fácil quanto a aplicação de um atributo Authorize ao método de ação (ou à classe do controlador todas as ações dele exigirem autorização), como mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="76d4d-107">Restricting access to an ASP.NET Core MVC route is as easy as applying an Authorize attribute to the action method (or to the controller’s class if all the controller’s actions require authorization), as shown in following example:</span></span>

```csharp
public class AccountController : Controller
{
    public ActionResult Login()
    {
    }

    [Authorize]
    public ActionResult Logout()
    {
    }
}
```

<span data-ttu-id="76d4d-108">Por padrão, adicionar um atributo Authorize sem parâmetros limitará o acesso aos usuários autenticados do controlador ou ação.</span><span class="sxs-lookup"><span data-stu-id="76d4d-108">By default, adding an Authorize attribute without parameters will limit access to authenticated users for that controller or action.</span></span> <span data-ttu-id="76d4d-109">Para restringir a disponibilidade de uma API apenas para usuários específicos, o atributo pode ser expandido para especificar funções necessárias ou políticas que os usuários devem atender.</span><span class="sxs-lookup"><span data-stu-id="76d4d-109">To further restrict an API to be available for only specific users, the attribute can be expanded to specify required roles or policies that users must satisfy.</span></span>

## <a name="implement-role-based-authorization"></a><span data-ttu-id="76d4d-110">Implementar a autorização baseada em função</span><span class="sxs-lookup"><span data-stu-id="76d4d-110">Implement role-based authorization</span></span>

<span data-ttu-id="76d4d-111">A identidade do ASP.NET Core tem um conceito interno de funções.</span><span class="sxs-lookup"><span data-stu-id="76d4d-111">ASP.NET Core Identity has a built-in concept of roles.</span></span> <span data-ttu-id="76d4d-112">Além dos usuários, a identidade do ASP.NET Core armazena informações sobre as diferentes funções usadas pelo aplicativo e controla quais usuários são atribuídos a quais funções.</span><span class="sxs-lookup"><span data-stu-id="76d4d-112">In addition to users, ASP.NET Core Identity stores information about different roles used by the application and keeps track of which users are assigned to which roles.</span></span> <span data-ttu-id="76d4d-113">Essas atribuições podem ser alteradas de forma programática com o tipo `RoleManager`, que atualiza funções no armazenamento persistente, e o tipo `UserManager`, que pode conceder ou revogar as funções de usuários.</span><span class="sxs-lookup"><span data-stu-id="76d4d-113">These assignments can be changed programmatically with the `RoleManager` type that updates roles in persisted storage, and the `UserManager` type that can grant or revoke roles from users.</span></span>

<span data-ttu-id="76d4d-114">Se você estiver autenticando com tokens de portador do JWT, o middleware de autenticação de portador do JWT do ASP.NET Core preencherá funções de usuário com base nas declarações de função encontradas no token.</span><span class="sxs-lookup"><span data-stu-id="76d4d-114">If you're authenticating with JWT bearer tokens, the ASP.NET Core JWT bearer authentication middleware will populate a user’s roles based on role claims found in the token.</span></span> <span data-ttu-id="76d4d-115">Para limitar o acesso a uma ação ou um controlador MVC a usuários em funções específicas, você pode incluir um parâmetro de funções na anotação Authorize (atributo), como mostra o seguinte fragmento de código:</span><span class="sxs-lookup"><span data-stu-id="76d4d-115">To limit access to an MVC action or controller to users in specific roles, you can include a Roles parameter in the Authorize annotation (attribute), as shown in the following code fragment:</span></span>

```csharp
[Authorize(Roles = "Administrator, PowerUser")]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [Authorize(Roles = "Administrator")]
    public ActionResult ShutDown()
    {
    }
}
```

<span data-ttu-id="76d4d-116">Neste exemplo, apenas os usuários nas funções Administrator ou PowerUser podem acessar APIs no controlador ControlPanel (como executar a ação SetTime).</span><span class="sxs-lookup"><span data-stu-id="76d4d-116">In this example, only users in the Administrator or PowerUser roles can access APIs in the ControlPanel controller (such as executing the SetTime action).</span></span> <span data-ttu-id="76d4d-117">A API ShutDown é mais restrita para permitir o acesso somente aos usuários na função Administrator.</span><span class="sxs-lookup"><span data-stu-id="76d4d-117">The ShutDown API is further restricted to allow access only to users in the Administrator role.</span></span>

<span data-ttu-id="76d4d-118">Para exigir que um usuário esteja em várias funções, use vários atributos Authorize, conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="76d4d-118">To require a user be in multiple roles, you use multiple Authorize attributes, as shown in the following example:</span></span>

```csharp
[Authorize(Roles = "Administrator, PowerUser")]
[Authorize(Roles = "RemoteEmployee ")]
[Authorize(Policy = "CustomPolicy")]
public ActionResult API1 ()
{
}
```

<span data-ttu-id="76d4d-119">Neste exemplo, para chamar API1, um usuário deve:</span><span class="sxs-lookup"><span data-stu-id="76d4d-119">In this example, to call API1, a user must:</span></span>

- <span data-ttu-id="76d4d-120">estar na função Administrator *ou* PowerUser *e*</span><span class="sxs-lookup"><span data-stu-id="76d4d-120">Be in the Administrator *or* PowerUser role, *and*</span></span>

- <span data-ttu-id="76d4d-121">estar na função RemoteEmployee *e*</span><span class="sxs-lookup"><span data-stu-id="76d4d-121">Be in the RemoteEmployee role, *and*</span></span>

- <span data-ttu-id="76d4d-122">satisfazer a um manipulador personalizado da autorização CustomPolicy.</span><span class="sxs-lookup"><span data-stu-id="76d4d-122">Satisfy a custom handler for CustomPolicy authorization.</span></span>

## <a name="implement-policy-based-authorization"></a><span data-ttu-id="76d4d-123">Implementar a autorização baseada em política</span><span class="sxs-lookup"><span data-stu-id="76d4d-123">Implement policy-based authorization</span></span>

<span data-ttu-id="76d4d-124">Regras de autorização personalizadas também podem ser gravadas usando [políticas de autorização](https://docs.asp.net/en/latest/security/authorization/policies.html).</span><span class="sxs-lookup"><span data-stu-id="76d4d-124">Custom authorization rules can also be written using [authorization policies](https://docs.asp.net/en/latest/security/authorization/policies.html).</span></span> <span data-ttu-id="76d4d-125">Esta seção fornece uma visão geral.</span><span class="sxs-lookup"><span data-stu-id="76d4d-125">This section provides an overview.</span></span> <span data-ttu-id="76d4d-126">Para obter mais informações, confira o [Workshop de autorização do ASP.NET](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="76d4d-126">For more information, see the [ASP.NET Authorization Workshop](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span>

<span data-ttu-id="76d4d-127">Políticas de autorização personalizadas são registradas no método Startup.ConfigureServices usando o método service.AddAuthorization.</span><span class="sxs-lookup"><span data-stu-id="76d4d-127">Custom authorization policies are registered in the Startup.ConfigureServices method using the service.AddAuthorization method.</span></span> <span data-ttu-id="76d4d-128">Esse método usa um delegado que configura um argumento AuthorizationOptions.</span><span class="sxs-lookup"><span data-stu-id="76d4d-128">This method takes a delegate that configures an AuthorizationOptions argument.</span></span>

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("AdministratorsOnly", policy =>
        policy.RequireRole("Administrator"));
    options.AddPolicy("EmployeesOnly", policy =>
        policy.RequireClaim("EmployeeNumber"));
    options.AddPolicy("Over21", policy =>
        policy.Requirements.Add(new MinimumAgeRequirement(21)));
});
```

<span data-ttu-id="76d4d-129">Conforme mostrado no exemplo, políticas podem ser associadas a diferentes tipos de requisitos.</span><span class="sxs-lookup"><span data-stu-id="76d4d-129">As shown in the example, policies can be associated with different types of requirements.</span></span> <span data-ttu-id="76d4d-130">Depois que as políticas forem registradas, elas poderão ser aplicadas a um controlador ou uma ação passando o nome da política como o argumento Policy do atributo Authorize (por exemplo, `[Authorize(Policy="EmployeesOnly")]`). As políticas podem ter vários requisitos, não apenas um (como mostram estes exemplos).</span><span class="sxs-lookup"><span data-stu-id="76d4d-130">After the policies are registered, they can be applied to an action or controller by passing the policy’s name as the Policy argument of the Authorize attribute (for example, `[Authorize(Policy="EmployeesOnly")]`) Policies can have multiple requirements, not just one (as shown in these examples).</span></span>

<span data-ttu-id="76d4d-131">No exemplo anterior, a primeira chamada AddPolicy é apenas uma maneira alternativa de autorizar pela função.</span><span class="sxs-lookup"><span data-stu-id="76d4d-131">In the previous example, the first AddPolicy call is just an alternative way of authorizing by role.</span></span> <span data-ttu-id="76d4d-132">Se `[Authorize(Policy="AdministratorsOnly")]` for aplicado a uma API, somente os usuários na função Administrator poderão acessá-lo.</span><span class="sxs-lookup"><span data-stu-id="76d4d-132">If `[Authorize(Policy="AdministratorsOnly")]` is applied to an API, only users in the Administrator role will be able to access it.</span></span>

<span data-ttu-id="76d4d-133">A segunda chamada de <xref:Microsoft.AspNetCore.Authorization.AuthorizationOptions.AddPolicy%2A> demonstra uma maneira fácil de exigir que uma determinada declaração esteja presente para o usuário.</span><span class="sxs-lookup"><span data-stu-id="76d4d-133">The second <xref:Microsoft.AspNetCore.Authorization.AuthorizationOptions.AddPolicy%2A> call demonstrates an easy way to require that a particular claim should be present for the user.</span></span> <span data-ttu-id="76d4d-134">O método <xref:Microsoft.AspNetCore.Authorization.AuthorizationPolicyBuilder.RequireClaim%2A> também usa os valores esperados para a declaração.</span><span class="sxs-lookup"><span data-stu-id="76d4d-134">The <xref:Microsoft.AspNetCore.Authorization.AuthorizationPolicyBuilder.RequireClaim%2A> method also optionally takes expected values for the claim.</span></span> <span data-ttu-id="76d4d-135">Se os valores são especificados, o requisito é atendido somente se o usuário tem uma declaração do tipo correto e um dos valores especificados.</span><span class="sxs-lookup"><span data-stu-id="76d4d-135">If values are specified, the requirement is met only if the user has both a claim of the correct type and one of the specified values.</span></span> <span data-ttu-id="76d4d-136">Se você estiver usando o middleware de autenticação de portador do JWT, todas as propriedades do JWT estarão disponíveis como declarações de usuário.</span><span class="sxs-lookup"><span data-stu-id="76d4d-136">If you're using the JWT bearer authentication middleware, all JWT properties will be available as user claims.</span></span>

<span data-ttu-id="76d4d-137">A política mais interessante mostrada aqui está no terceiro método `AddPolicy`, porque ele usa um requisito de autorização personalizado.</span><span class="sxs-lookup"><span data-stu-id="76d4d-137">The most interesting policy shown here is in the third `AddPolicy` method, because it uses a custom authorization requirement.</span></span> <span data-ttu-id="76d4d-138">Usando os requisitos de autorização personalizados, você pode ter grande controle sobre como a autorização é realizada.</span><span class="sxs-lookup"><span data-stu-id="76d4d-138">By using custom authorization requirements, you can have a great deal of control over how authorization is performed.</span></span> <span data-ttu-id="76d4d-139">Para que isso funcione, você deve implementar esses tipos:</span><span class="sxs-lookup"><span data-stu-id="76d4d-139">For this to work, you must implement these types:</span></span>

- <span data-ttu-id="76d4d-140">Um tipo Requirements que é derivado de <xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> e contém campos para especificar os detalhes do requisito.</span><span class="sxs-lookup"><span data-stu-id="76d4d-140">A Requirements type that derives from <xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> and that contains fields specifying the details of the requirement.</span></span> <span data-ttu-id="76d4d-141">No exemplo, esse é um campo de idade para o tipo `MinimumAgeRequirement` de exemplo.</span><span class="sxs-lookup"><span data-stu-id="76d4d-141">In the example, this is an age field for the sample `MinimumAgeRequirement` type.</span></span>

- <span data-ttu-id="76d4d-142">Um manipulador que implementa <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandler%601>, em que T é o tipo de <xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> que o manipulador pode atender.</span><span class="sxs-lookup"><span data-stu-id="76d4d-142">A handler that implements <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandler%601>, where T is the type of <xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> that the handler can satisfy.</span></span> <span data-ttu-id="76d4d-143">O manipulador precisa implementar o método <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandler%601.HandleRequirementAsync%2A>, que verifica se um contexto especificado que contém informações sobre o usuário atende ao requisito.</span><span class="sxs-lookup"><span data-stu-id="76d4d-143">The handler must implement the <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandler%601.HandleRequirementAsync%2A> method, which checks whether a specified context that contains information about the user satisfies the requirement.</span></span>

<span data-ttu-id="76d4d-144">Se o usuário atender ao requisito, uma chamada para `context.Succeed` indicará que o usuário está autorizado.</span><span class="sxs-lookup"><span data-stu-id="76d4d-144">If the user meets the requirement, a call to `context.Succeed` will indicate that the user is authorized.</span></span> <span data-ttu-id="76d4d-145">Se houver várias maneiras pelas quais um usuário pode satisfazer a um requisito de autorização, vários manipuladores poderão ser criados.</span><span class="sxs-lookup"><span data-stu-id="76d4d-145">If there are multiple ways that a user might satisfy an authorization requirement, multiple handlers can be created.</span></span>

<span data-ttu-id="76d4d-146">Além de registrar requisitos de política personalizada com chamadas de `AddPolicy`, você também precisa registrar manipuladores de requisito personalizados por meio da injeção de dependência (`services.AddTransient<IAuthorizationHandler, MinimumAgeHandler>()`).</span><span class="sxs-lookup"><span data-stu-id="76d4d-146">In addition to registering custom policy requirements with `AddPolicy` calls, you also need to register custom requirement handlers via Dependency Injection (`services.AddTransient<IAuthorizationHandler, MinimumAgeHandler>()`).</span></span>

<span data-ttu-id="76d4d-147">Um exemplo de requisito de autorização personalizado e de manipulador para verificar a idade do usuário (com base em uma declaração `DateOfBirth`) está disponível na [documentação de autorização](https://docs.asp.net/en/latest/security/authorization/policies.html) do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="76d4d-147">An example of a custom authorization requirement and handler for checking a user’s age (based on a `DateOfBirth` claim) is available in the ASP.NET Core [authorization documentation](https://docs.asp.net/en/latest/security/authorization/policies.html).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="76d4d-148">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="76d4d-148">Additional resources</span></span>

- <span data-ttu-id="76d4d-149">**Autenticação do ASP.NET Core** </span><span class="sxs-lookup"><span data-stu-id="76d4d-149">**ASP.NET Core Authentication** </span></span>\
  [https://docs.microsoft.com/aspnet/core/security/authentication/identity](/aspnet/core/security/authentication/identity)

- <span data-ttu-id="76d4d-150">**Autorização do ASP.NET Core** </span><span class="sxs-lookup"><span data-stu-id="76d4d-150">**ASP.NET Core Authorization** </span></span>\
  [https://docs.microsoft.com/aspnet/core/security/authorization/introduction](/aspnet/core/security/authorization/introduction)

- <span data-ttu-id="76d4d-151">**Autorização baseada em função** </span><span class="sxs-lookup"><span data-stu-id="76d4d-151">**Role-based Authorization** </span></span>\
  [https://docs.microsoft.com/aspnet/core/security/authorization/roles](/aspnet/core/security/authorization/roles)

- <span data-ttu-id="76d4d-152">**Autorização baseada em política personalizada** </span><span class="sxs-lookup"><span data-stu-id="76d4d-152">**Custom Policy-Based Authorization** </span></span>\
  [https://docs.microsoft.com/aspnet/core/security/authorization/policies](/aspnet/core/security/authorization/policies)

>[!div class="step-by-step"]
><span data-ttu-id="76d4d-153">[Anterior](index.md)
>[Próximo](developer-app-secrets-storage.md)</span><span class="sxs-lookup"><span data-stu-id="76d4d-153">[Previous](index.md)
[Next](developer-app-secrets-storage.md)</span></span>