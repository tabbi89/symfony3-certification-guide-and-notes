# Security

#### Certification requirements

* [Authentication](#authentication)
* [Authorization](#authorization)

#### Extras

* Katas

## Authentication <a id="authentication"></a>

#### Links

* <http://symfony.com/doc/3.0/components/security/authentication.html>
* <http://symfony.com/doc/3.0/book/security.html#how-security-works-authentication-and-authorization>
* <http://symfony.com/doc/3.0/cookbook/security/index.html#authentication-identifying-logging-in-the-user>

#### You should know

* that Authentication is about Identifying/Logging in the User
* [flow of the firewall][a-3]
		
		firewall is registered as a listener on the kernel.request event
		each listener authenticate a user, throw AuthenticationException or skip 
		(no authentication information on Request)
* [main listener `Symfony\Component\Security\Http\Firewall` for `kernel.request` which register other firewall listeners][a-4]
	* [Firewall listeners][a-5]
* [when the user is not authenticated at all (no token in token storage) firewall entry point is being called][a-6]
* [if user is not authenticated `AnonymousToken` is created in token storage][a-7]

		$user = $this->get('security.token_storage')->getToken()->getUser();
		\\ returns string anon.
		
	* [	that is way controller->getUser() returns null or object][a-8]
* [all authentication events][a-1]
	
		security.authentication.success - AuthenticationEvent
		security.authentication.failure - AuthenticationFailureEvent
		security.interactive_login - InteractiveLoginEvent
		security.switch_user - SwitchUserEvent
	
* [that `security.interactive_login` is triggered when user has actively logged into app][a-2]

[a-1]: http://symfony.com/doc/3.0/components/security/authentication.html#authentication-events
[a-2]: http://symfony.com/doc/3.0/components/security/authentication.html#security-events
[a-3]: https://symfony.com/doc/3.0/components/security/firewall.html#flow-firewall-authentication-authorization
[a-4]: http://api.symfony.com/3.0/Symfony/Component/Security/Http/Firewall.html
[a-5]: https://github.com/symfony/security-http/tree/3.0/Firewall
[a-6]: https://symfony.com/doc/3.0/components/security/firewall.html#entry-points
[a-7]: https://github.com/symfony/security-http/blob/3.0/Firewall/AnonymousAuthenticationListener.php
[a-8]: https://github.com/symfony/framework-bundle/blob/3.0/Controller/Controller.php#L324

## Authorization <a id="authorization"></a>

#### Links

* <http://symfony.com/doc/3.0/components/security/authorization.html>
* <http://symfony.com/doc/3.0/book/security.html#authorization>
* <http://symfony.com/doc/3.0/cookbook/security/index.html#authorization-denying-access>

#### You should know

* [that decision if user can have access to this resource is made by `AccessDecisionManagerInterface` -> `decide(TokenInterface $token, array $attributes, object $object = null)`][au-1]
* [that strategies in voters][au-2]
		affirmative (default) >= 1
		consensus > more
		unanimous = all
		
* that `VoterInterface` has method `vote(TokenInterface $token, $object, array $attributes)`
		returns:
		
		VoterInterface::ACCESS_GRANTED = 1
		VoterInterface::ACCESS_DENIED = -1
		VoterInterface::ACCESS_ABSTAIN = 0
		
* that there is built-in `AuthenticatedVoter` and supports the attributes IS_AUTHENTICATED_FULLY, IS_AUTHENTICATED_REMEMBERED IS_AUTHENTICATED_ANONYMOUSLY
* that there is built-in `RoleVoter` and supports attributes starting with `ROLE_*`
* that there is built-in `RoleHierarchyVoter`
* [that you can change the default strategy][au-3]
* [that a custom voter needs to implement `VoterInterface` or extend `Voter`][au-4]
		
		abstract class Voter implements VoterInterface
		{
		    abstract protected function supports($attribute, $subject);
		    abstract protected function voteOnAttribute($attribute, $subject, TokenInterface $token);
		}
		
[au-1]: http://api.symfony.com/3.0/Symfony/Component/Security/Core/Authorization/AccessDecisionManagerInterface.html
[au-2]: http://symfony.com/doc/3.0/components/security/authorization.html#access-decision-manager
[au-3]: http://symfony.com/doc/3.0/security/voters.html#changing-the-access-decision-strategy
[au-4]: http://symfony.com/doc/3.0/security/voters.html#the-voter-interface








