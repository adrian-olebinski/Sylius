# UPGRADE FROM `1.13` TO `2.0`

* API Platform has dropped `DataProviders` and `DataPersisters` in favor of `Providers` and `Processors`, respectively.
  Due to this change, Sylius custom `DataProviders` and `DataPersisters` have been adapted to the new API Platform interfaces
  and their namespaced have been changed to `StateProvider` and `StateProcessor` respectively:
- `Sylius\Bundle\ApiBundle\DataPersister\*DataPersister` => `Sylius\Bundle\ApiBundle\StateProcessor\*Processor`
- `Sylius\Bundle\ApiBundle\DataProvider\*DataProvider` => `Sylius\Bundle\ApiBundle\StateProvider\*Provider`

* The parameter type and order of the `Sylius\Bundle\ApiBundle\Controller\UploadAvatarImageAction::__construct` has been changed:
```php
    public function __construct(
        private FactoryInterface $avatarImageFactory,
        private AvatarImageRepositoryInterface $avatarImageRepository,
    -   private ImageUploaderInterface $imageUploader,
    -   private IriConverterInterface $iriConverter,
    +   private RepositoryInterface $adminUserRepository,
    +   private ImageUploaderInterface $imageUploader,
    )
```

* Updated API routes related to avatar management:

  Previous Routes:
    * `'GET' - /api/v2/admin/avatar-images/{id}`
    * `'POST' - /api/v2/admin/avatar-images`
    * `'DELETE' - /api/v2/admin/avatar-images/{id}`

  New Routes:
    * `'GET' - /api/v2/admin/administrators/{id}/avatar-image`
    * `'POST' - /api/v2/admin/administrators/{id}/avatar-image`
    * `'DELETE' - /api/v2/admin/administrators/{id}/avatar-image`

* Updated API routes related to shop user management:

  Previous Routes:
    * `'POST' - /api/v2/shop/reset-password-requests`
    * `'PATCH' - /api/v2/shop/reset-password-requests/{resetPasswordToken}`

  New Routes:
    * `'POST' - /api/v2/shop/reset-password`
    * `'PATCH' - /api/v2/shop/reset-password/{resetPasswordToken}`

* The `getCurrentPrefix` method has been removed from the `Sylius\Bundle\ApiBundle\Provider\PathPrefixProviderInterface`.

* The `Sylius\Bundle\ApiBundle\Provider\PathPrefixProvider` constructor has been changed due to refactor. 
  Now, we provide the list of possible prefixes that we check in this service. This list can be set under 
  the parameter: `sylius.api_path_prefixes`. 

    ```diff
        public function __construct(
    -       private UserContextInterface $userContext,
            private string $apiRoute,
    +       private array $pathPrefixes,
        ) {
            ...
        }
    ```