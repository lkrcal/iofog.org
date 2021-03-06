# Controller REST API Reference

<swagger-ui>
  swagger: '2.0'
  info:
    version: 3.0.0
    title: FogController
  consumes:
  - application/json
  produces:
  - application/json
  paths:
    '/status':
      get:
        tags:
        - Controller
        description: Returns service health status
        operationId: getServiceStatus
        responses:
          '200':
            description: Service status
            schema:
              $ref: '#/definitions/ServiceStatusResponse'
          '500':
            description: Internal Server Error
    '/email-activation':
      get:
        tags:
        - Controller
        description: Returns email activation status
        operationId: getEmailActivationStatus
        responses:
          '200':
            description: Email activation status
            schema:
              $ref: '#/definitions/EmailActivationStatusResponse'
          '500':
            description: Internal Server Error
    '/fog-types':
      get:
        tags:
        - Controller
        description: Gets ioFog types list
        operationId: getIOFogTypes
        responses:
          '200':
            description: Success
            schema:
              $ref: '#/definitions/IOFogTypesResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '500':
            description: Internal Server Error
    '/iofog-list':
      post:
        tags:
        - ioFog
        description: Returns list of ioFog nodes
        operationId: getIOFogNodes
        parameters:
        - in: header
          name: Authorization
          description: User token
          required: true
          type: string
        - in: body
          name: Filters
          required: false
          schema:
            $ref: '#/definitions/IOFogNodesListFilters'
        responses:
          '200':
            description: List of ioFog nodes
            schema:
              $ref: '#/definitions/IOFogNodesListResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Bad Request
          '401':
            description: Not Authorized
          '500':
            description: Internal Server Error
    /iofog:
      post:
        tags:
        - ioFog
        description: Creates a new ioFog node
        operationId: createIOFogNode
        parameters:
        - in: header
          name: Authorization
          description: User token
          required: true
          type: string
        - in: body
          name: IOFogNodeInfo
          required: true
          schema:
            $ref: '#/definitions/UpdateIOFogNodeRequestBody'
        responses:
          '201':
            description: Created
            schema:
              $ref: '#/definitions/NewIOFogNodeResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Bad Request
          '401':
            description: Not Authorized
          '500':
            description: Internal Server Error
    '/iofog/{uuid}':
      patch:
        tags:
        - ioFog
        description: Updates existing ioFog node
        operationId: updateIOFogNode
        parameters:
        - in: path
          name: uuid
          description: ioFog node id
          required: true
          type: string
        - in: header
          name: Authorization
          description: User token
          required: true
          type: string
        - in: body
          name: ioFogNodeInfo
          required: true
          schema:
            $ref: '#/definitions/UpdateIOFogNodeRequestBody'
        responses:
          '204':
            description: Updated
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Bad Request
          '401':
            description: Not Authorized
          '404':
            description: Invalid Node Id
          '500':
            description: Internal Server Error
      delete:
        tags:
        - ioFog
        description: Deletes an ioFog node
        operationId: deleteIOFogNode
        parameters:
        - in: path
          name: uuid
          description: ioFog node id
          required: true
          type: string
        - in: header
          name: Authorization
          description: User token
          required: true
          type: string
        responses:
          '202':
            description: Accepted
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '404':
            description: Invalid Node Id
          '500':
            description: Internal Server Error
      get:
        tags:
        - ioFog
        description: Gets ioFog node info
        operationId: getIOFogNode
        parameters:
        - in: path
          name: uuid
          description: ioFog node id
          required: true
          type: string
        - in: header
          name: Authorization
          description: User token
          required: true
          type: string
        responses:
          '200':
            description: Success
            schema:
              $ref: '#/definitions/IOFogNodeInfoResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '404':
            description: Invalid Node Id
          '500':
            description: Internal Server Error
    '/iofog/{uuid}/provisioning-key':
      get:
        tags:
        - ioFog
        description: Generates provisioning key for an ioFog node
        operationId: generateProvisioningKey
        parameters:
        - in: path
          name: uuid
          description: ioFog node id
          required: true
          type: string
        - in: header
          name: Authorization
          description: User token
          required: true
          type: string
        responses:
          '201':
            description: Success
            schema:
              $ref: '#/definitions/ProvisioningKeyResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '404':
            description: Invalid Node Id
          '500':
            description: Internal Server Error
    '/iofog/{uuid}/version/{versionCommand}':
      post:
        tags:
        - ioFog
        description: Set change version command
        operationId: setVersionCommand
        parameters:
        - in: header
          name: Authorization
          description: User token
          required: true
          type: string
        - in: path
          name: uuid
          description: ioFog node id
          required: true
          type: string
        - in: path
          name: versionCommand
          description: change version command
          required: true
          type: string
          enum:
          - upgrade
          - rollback
        responses:
          204:
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          400:
            description: Bad Request
          401:
            description: Not Authorized
          404:
            description: Invalid Node Id
          500:
            description: Internal Server Error
    '/iofog/{uuid}/reboot':
      post:
        tags:
        - ioFog
        description: remote reboot fog agent
        operationId: setRebootCommand
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: path
          name: uuid
          description: ioFog node id
          required: true
          type: string
        responses:
          204:
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          401:
            description: Not Authorized
          500:
            description: Internal Server Error
    '/iofog/{uuid}/hal/hw':
      get:
        tags:
        - ioFog
        description: Retrieves HAL hardware info
        operationId: getFogHalHardwareInfo
        parameters:
        - in: path
          name: uuid
          description: ioFog node id
          required: true
          type: string
        - in: header
          name: Authorization
          description: User token
          required: true
          type: string
        responses:
          '200':
            description: Success
            schema:
              $ref: '#/definitions/HalInfo'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '404':
            description: Invalid Node Id
          '500':
            description: Internal Server Error
    '/iofog/{uuid}/hal/usb':
      get:
        tags:
        - ioFog
        description: Retrieves HAL USB info
        operationId: getFogHalUsbInfo
        parameters:
        - in: path
          name: uuid
          description: ioFog node id
          required: true
          type: string
        - in: header
          name: Authorization
          description: User token
          required: true
          type: string
        responses:
          '200':
            description: Success
            schema:
              $ref: '#/definitions/HalInfo'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '404':
            description: Invalid Node Id
          '500':
            description: Internal Server Error
    '/agent/provision':
      post:
        tags:
        - Agent
        description: Provision agent with an ioFog node
        operationId: agentProvision
        parameters:
        - in: body
          required: true
          name: AgentProvisioningRequest
          schema:
            $ref: '#/definitions/AgentProvisioningRequest'
        responses:
          '200':
            description: Success
            schema:
              $ref: '#/definitions/AgentProvisioningResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Bad Request
          '401':
            description: Expired Provisioning Key
          '404':
            description: Invalid Provisioning Key
          '500':
            description: Internal Server Error
    '/agent/deprovision':
      post:
        tags:
        - Agent
        description: Deprovision agent
        operationId: agentDeprovision
        parameters:
        - in: body
          required: true
          name: AgentDeprovisioningRequest
          schema:
            $ref: '#/definitions/AgentDeprovisioningRequest'
        responses:
          '204':
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Bad Request
          '401':
            description: Not Authorized
          '500':
            description: Internal Server Error
    '/agent/config':
      get:
        tags:
        - Agent
        description: Get an ioFog node configuration
        operationId: getIOFogNodeConfig
        parameters:
        - in: header
          name: Authorization
          description: Agent Token
          required: true
          type: string
        responses:
          '200':
            description: Success
            schema:
              $ref: '#/definitions/IOFogNodeConfig'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '500':
            description: Internal Server Error
      patch:
        tags:
        - Agent
        description: Updates an ioFog node configuration
        operationId: updateIOFogNodeConfig
        parameters:
        - in: header
          name: Authorization
          description: Agent Token
          required: true
          type: string
        - in: body
          name: UpdateIOFogNodeConfigRequestBody
          required: true
          schema:
            $ref: '#/definitions/IOFogNodeConfigRequest'
        responses:
          '204':
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Bad Request
          '401':
            description: Not Authorized
          '500':
            description: Internal Server Error
    '/agent/config/changes':
      get:
        tags:
        - Agent
        description: Gets ioFog node changes
        operationId: getIOFogNodeChanges
        parameters:
        - in: header
          name: Authorization
          description: Agent Token
          required: true
          type: string
        responses:
          '200':
            description: Success
            schema:
              $ref: '#/definitions/IOFogNodeConfigChanges'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Bad Request
          '401':
            description: Not Authorized
          '500':
            description: Internal Server Error
    '/agent/status':
      put:
        tags:
        - Agent
        description: Posts agent status to ioFog node
        operationId: postAgentStatus
        parameters:
        - in: header
          name: Authorization
          description: Agent Token
          required: true
          type: string
        - in: body
          name: AgentStatus
          required: true
          schema:
            $ref: '#/definitions/AgentStatus'
        responses:
          '204':
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Bad Request
          '401':
            description: Not Authorized
          '500':
            description: Internal Server Error
    '/agent/microservices':
      get:
        tags:
        - Agent
        description: Gets microservices running on an ioFog node
        operationId: getAgentMicroservicesList
        parameters:
        - in: header
          name: Authorization
          description: Agent Token
          required: true
          type: string
        responses:
          '200':
            description: Success
            schema:
              $ref: '#/definitions/AgentMicroservicesListResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '500':
            description: Internal Server Error
    '/agent/microservices/{microserviceUuid}':
      get:
        tags:
        - Agent
        description: Gets microservices running on an ioFog node
        operationId: getAgentMicroserviceInfo
        parameters:
        - in: path
          required: true
          name: microserviceUuid
          description: Microservice UUID
          type: string
        - in: header
          name: Authorization
          description: Agent Token
          required: true
          type: string
        responses:
          '200':
            description: Success
            schema:
              $ref: '#/definitions/AgentMicroservicesInfoResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '404':
            description: Invalid Microservice Uuid
          '500':
            description: Internal Server Error
    '/agent/registries':
      get:
        tags:
        - Agent
        description: Gets list of Docker registries
        operationId: getRegistriesList
        parameters:
        - in: header
          name: Authorization
          description: Agent Token
          required: true
          type: string
        responses:
          '200':
            description: Success
            schema:
              $ref: '#/definitions/RegistriesListResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '500':
            description: Internal Server Error
    '/agent/tunnel':
      get:
        tags:
        - Agent
        description: Get an ioFog node tunnel configuration
        operationId: getIOFogNodeTunnelConfig
        parameters:
        - in: header
          name: Authorization
          description: Agent Token
          required: true
          type: string
        responses:
          '200':
            description: Success
            schema:
              $ref: '#/definitions/IOFogNodeTunnelConfigResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '404':
            description: Tunnel Not Found
          '500':
            description: Internal Server Error
    '/agent/strace':
      get:
        tags:
        - Agent
        description: Get an ioFog node strace info
        operationId: getIOFogNodeStraceInfo
        parameters:
        - in: header
          name: Authorization
          description: Agent Token
          required: true
          type: string
        responses:
          '200':
            description: Success
            schema:
              $ref: '#/definitions/IOFogNodeStraceResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '404':
            description: Strace Not Found
          '500':
            description: Internal Server Error
      put:
        tags:
        - Agent
        description: Posts agent strace to ioFog node
        operationId: postIOFogNodeStraceBuffer
        parameters:
        - in: header
          name: Authorization
          description: Agent Token
          required: true
          type: string
        - in: body
          name: AgentStraceBuffers
          required: true
          schema:
            $ref: '#/definitions/IOFogNodeStraceBuffer'
        responses:
          '204':
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Bad Request
          '401':
            description: Not Authorized
          '404':
            description: Invalid Node Id
          '500':
            description: Internal Server Error
    '/agent/version':
      get:
        tags:
        - Agent
        description: Get change version command
        operationId: getChangeVersion
        parameters:
        - in: header
          name: Authorization
          description: Agent Token
          required: true
          type: string
        responses:
          200:
            description: Success
            schema:
              $ref: '#/definitions/VersionCommandResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          401:
            description: Not Authorized
          404:
            description: Version Command Not Found
          500:
            description: Internal Server Error
    '/agent/hal/hw':
      put:
        tags:
        - Agent
        description: Updates HAL hardware info
        operationId: putHalHardwareInfo
        parameters:
        - in: header
          name: Authorization
          description: Agent Token
          required: true
          type: string
        - in: body
          name: HalInfo
          required: true
          schema:
            $ref: '#/definitions/HalInfo'
        responses:
          '204':
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Bad Request
          '401':
            description: Not Authorized
          '500':
            description: Internal Server Error
    '/agent/hal/usb':
      put:
        tags:
        - Agent
        description: Updates HAL USB info
        operationId: putHalUsbInfo
        parameters:
        - in: header
          name: Authorization
          description: Agent Token
          required: true
          type: string
        - in: body
          name: HalInfo
          required: true
          schema:
            $ref: '#/definitions/HalInfo'
        responses:
          '204':
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Bad Request
          '401':
            description: Not Authorized
          '500':
            description: Internal Server Error
    '/agent/delete-node':
      delete:
        tags:
        - Agent
        description: Deletes an ioFog node
        operationId: deleteAgentNode
        parameters:
        - in: header
          name: Authorization
          description: Agent token
          required: true
          type: string
        responses:
          '204':
            description: No Content
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '500':
            description: Internal Server Error
    '/agent/image-snapshot':
      get:
        tags:
        - Agent
        description: Get image snapshot info
        operationId: getImageSnapshot
        parameters:
        - in: header
          name: Authorization
          description: Agent Token
          required: true
          type: string
        responses:
          200:
            description: Success
            schema:
              $ref: '#/definitions/ImageSnapshotResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          401:
            description: Not Authorized
          404:
            description: Image Snapshot Not Found
          500:
            description: Internal Server Error
      put:
        tags:
        - Agent
        description: Put image snapshot info on controller
        operationId: putImageSnapshot
        parameters:
        - in: header
          name: Authorization
          description: Agent Token
          required: true
          type: string
        - in: body
          name: ImageSnapshotRequest
          required: true
          schema:
            $ref: '#/definitions/ImageSnapshotRequest'
        responses:
          '204':
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Bad Request
          '401':
            description: Not Authorized
          '500':
            description: Internal Server Error
    '/agent/tracking':
      post:
        tags:
        - Agent
        description: Post tracking info
        operationId: postTracking
        parameters:
        - in: header
          name: Authorization
          description: Agent Token
          required: true
          type: string
        - in: body
          name: PostTrackingRequest
          required: true
          schema:
            $ref: '#/definitions/PostTrackingRequest'  
        responses:
          204:
            description: Success
          401:
            description: Not Authorized
          500:
            description: Internal Server Error
    /catalog/microservices:
      get:
        tags:
        - Catalog
        description: Gets microservices catalog
        operationId: getMicroservicesCatalog
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        responses:
          '200':
            description: Success
            schema:
              $ref: '#/definitions/MicroservicesCatalogResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '500':
            description: Internal Server Error
      post:
        tags:
        - Catalog
        description: Creates a new microservice catalog item
        operationId: createMicroserviceCatalogItem
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: body
          name: CreateCatalogItem
          description: Microservice Catalog Item Info
          required: true
          schema:
            $ref: '#/definitions/CreateUpdateCatalogItemRequestBody'
        responses:
          '201':
            description: Created
            schema:
              type: object
              properties:
                id:
                  type: string
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Bad Request
          '401':
            description: Not Authorized
          '409':
            description: Duplicate Name
          '500':
            description: Internal Server Error
    '/catalog/microservices/{id}':
      get:
        tags:
        - Catalog
        description: Gets microservice catalog item info
        operationId: getMicroserviceCatalogItem
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: path
          name: id
          description: Catalog Item Id
          required: true
          type: string
        responses:
          '200':
            description: Catalog Item Info
            schema:
              $ref: '#/definitions/CatalogItemInfoResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '404':
            description: Invalid Catalog Item Id
          '500':
            description: Internal Server Error
      patch:
        tags:
        - Catalog
        description: Updates a microservice catalog item
        operationId: updateMicroserviceCatalogItem
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: path
          name: id
          description: Catalog Item Id
          required: true
          type: string
        - in: body
          name: UpdateCatalogItem
          description: Microservice Catalog Item Info
          required: true
          schema:
            $ref: '#/definitions/CreateUpdateCatalogItemRequestBody'
        responses:
          '204':
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Bad Request
          '401':
            description: Not Authorized
          '404':
            description: Invalid Catalog Item Id
          '409':
            description: Duplicate Name
          '500':
            description: Internal Server Error
      delete:
        tags:
        - Catalog
        description: Deletes a microservice catalog item
        operationId: deleteMicroserviceCatalogItem
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: path
          name: id
          description: Catalog Item Id
          required: true
          type: string
        responses:
          '204':
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '404':
            description: Invalid Catalog Item Id
          '500':
            description: Internal Server Error
    /flow:
      get:
        tags:
        - Flow
        description: Gets list of flows
        operationId: getFlowsList
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        responses:
          '200':
            description: Success
            schema:
              $ref: '#/definitions/GetFlowsResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '500':
            description: Internal Server Error
      post:
        tags:
        - Flow
        description: Creates a new flow
        operationId: createFlow
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: body
          name: NetFlowInfo
          description: New Flow Info
          required: true
          schema:
            $ref: '#/definitions/NewFlowRequest'
        responses:
          '201':
            description: Created
            schema:
              type: object
              properties:
                id:
                  type: string
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Bad Request
          '401':
            description: Not Authorized
          '500':
            description: Internal Server Error
    '/flow/{id}':
      get:
        tags:
        - Flow
        description: Gets flow info
        operationId: getFlowInfo
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: path
          name: id
          description: Flow Id
          required: true
          type: string
        responses:
          '200':
            description: Success
            schema:
              $ref: '#/definitions/FlowInfoResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '404':
            description: Invalid Flow Id
          '500':
            description: Internal Server Error
      patch:
        tags:
        - Flow
        description: Updates a flow
        operationId: updateFlow
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: path
          name: id
          description: Flow Id
          required: true
          type: string
        - in: body
          name: NetFlowInfo
          description: New Flow Info
          required: true
          schema:
            $ref: '#/definitions/NewFlowRequest'
        responses:
          '204':
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Bad Request
          '401':
            description: Not Authorized
          '404':
            description: Invalid Flow Id
          '500':
            description: Internal Server Error
      delete:
        tags:
        - Flow
        description: Deletes a flow
        operationId: deleteFlow
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: path
          name: id
          description: Flow Id
          required: true
          type: string
        responses:
          '204':
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '404':
            description: Invalid Flow Id
          '500':
            description: Internal Server Error
    /microservices:
      get:
        tags:
        - Microservices
        description: Gets list of microservices
        operationId: getMicroservicesList
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: query
          name: flowId
          description: Flow Id
          required: true
          type: integer
        responses:
          '200':
            description: Success
            schema:
              $ref: '#/definitions/GetMicroservicesResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '500':
            description: Internal Server Error
      post:
        tags:
        - Microservices
        description: Creates a new microservice on an ioFog node
        operationId: createMicroservice
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: body
          name: NewMicroserviceInfo
          description: New Microservice Info
          required: true
          schema:
            $ref: '#/definitions/NewMicroserviceRequest'
        responses:
          '201':
            description: Created
            schema:
              type: object
              properties:
                uuid:
                  type: string
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Bad Request
          '401':
            description: Not Authorized
          '409':
            description: Duplicate Name
          '500':
            description: Internal Server Error
    '/microservices/{uuid}':
      get:
        tags:
        - Microservices
        description: Gets a microservice info
        operationId: getMicroserviceInfo
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: path
          name: uuid
          description: Microservice Uuid
          required: true
          type: string
        responses:
          '200':
            description: Success
            schema:
              $ref: '#/definitions/MicroserviceInfoResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '404':
            description: Not Found
          '500':
            description: Internal Server Error
      patch:
        tags:
        - Microservices
        description: Updates a microservice
        operationId: updateMicroservice
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: path
          name: uuid
          description: Microservice Uuid
          required: true
          type: string
        - in: body
          name: UpdateMicroserviceInfo
          description: Microservice Info
          required: true
          schema:
            $ref: '#/definitions/UpdateMicroserviceRequest'
        responses:
          '204':
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Bad Request
          '401':
            description: Not Authorized
          '404':
            description: Not Found
          '409':
            description: Duplicate Name
          '500':
            description: Internal Server Error
      delete:
        tags:
        - Microservices
        description: Deletes a microservice
        operationId: deleteMicroservice
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: path
          name: uuid
          description: Microservice Uuid
          required: true
          type: string
        - in: body
          name: WithCleanupOption
          description: Delete option
          required: false
          schema:
            type: object
            properties:
              withCleanup:
                type: boolean
        responses:
          '204':
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Bad Request
          '401':
            description: Not Authorized
          '404':
            description: Not Found
          '500':
            description: Internal Server Error
    '/microservices/{uuid}/routes/{receiverUuid}':
      post:
        tags:
        - Microservices
        description: Creates a route from microservice to receiver
        operationId: createMicroserviceRoute
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: path
          name: uuid
          description: Microservice Uuid
          required: true
          type: string
        - in: path
          name: receiverUuid
          description: Receiver Microservice Uuid
          required: true
          type: string
        responses:
          '204':
            description: Created
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Not Valid
          '401':
            description: Not Authorized
          '404':
            description: Not Found
          '500':
            description: Internal Server Error
      delete:
        tags:
        - Microservices
        description: Deletes a route microservice
        operationId: deleteMicroserviceRoute
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: path
          name: uuid
          description: Microservice Uuid
          required: true
          type: string
        - in: path
          name: receiverUuid
          description: Receiver Microservice Uuid
          required: true
          type: string
        responses:
          '204':
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Not Valid
          '401':
            description: Not Authorized
          '404':
            description: Not Found
          '500':
            description: Internal Server Error
    '/microservices/{uuid}/port-mapping':
      post:
        tags:
        - Microservices
        description: Creates a port mapping for microservice
        operationId: createMicroservicePortMapping
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: path
          name: uuid
          description: Microservice Uuid
          required: true
          type: string
        - in: body
          name: portMappingData
          description: information about port mapping
          required: true
          schema:
            $ref: '#/definitions/PortMappingsRequest'
        responses:
          '201':
            description: Created
            schema:
              $ref: '#/definitions/PortMappingsPublicResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Not Valid
          '401':
            description: Not Authorized
          '404':
            description: Not Found
          '500':
            description: Internal Server Error
      get:
        tags:
        - Microservices
        description: Get a port mapping list for microservice
        operationId: getMicroservicePortMapping
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: path
          name: uuid
          description: Microservice Uuid
          required: true
          type: string
        responses:
          '200':
            description: Created
            schema:
              $ref: '#/definitions/PortMappingsListResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '404':
            description: Not Found
          '500':
            description: Internal Server Error
    '/microservices/{uuid}/port-mapping/{internalPort}':
      delete:
        tags:
        - Microservices
        description: Deletes a port mapping for microservice
        operationId: deleteMicroservicePortMapping
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: path
          name: uuid
          description: Microservice Uuid
          required: true
          type: string
        - in: path
          name: internalPort
          description: Internal Port
          required: true
          type: string
        responses:
          '204':
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '404':
            description: Not Found
          '500':
            description: Internal Server Error
    '/microservices/{uuid}/volume-mapping':
      post:
        tags:
        - Microservices
        description: Creates a volume mapping for microservice
        operationId: createMicroserviceVolumeMapping
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: path
          name: uuid
          description: Microservice Uuid
          required: true
          type: string
        - in: body
          name: volumeMappingData
          description: information about volume mapping
          required: true
          schema:
            $ref: '#/definitions/VolumeMapping'
        responses:
          '201':
            description: Created
            schema:
              type: object
              properties:
                id:
                  type: number
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Not Valid
          '401':
            description: Not Authorized
          '404':
            description: Not Found
          '500':
            description: Internal Server Error
      get:
        tags:
        - Microservices
        description: Get a volume mapping list for microservice
        operationId: getMicroserviceVolumeMapping
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: path
          name: uuid
          description: Microservice Uuid
          required: true
          type: string
        responses:
          '200':
            description: Success
            schema:
              $ref: '#/definitions/VolumeMappingResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '404':
            description: Not Found
          '500':
            description: Internal Server Error
    '/microservices/{uuid}/volume-mapping/{id}':
      delete:
        tags:
        - Microservices
        description: Deletes a volume mapping for microservice
        operationId: deleteMicroserviceVolumeMapping
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: path
          name: uuid
          description: Microservice Uuid
          required: true
          type: string
        - in: path
          name: id
          description: Volume id
          required: true
          type: string
        responses:
          '204':
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Not Valid
          '401':
            description: Not Authorized
          '404':
            description: Not Found
          '500':
            description: Internal Server Error
    '/microservices/{uuid}/image-snapshot':
      post:
        tags:
        - Diagnostics
        description: Send request to create image snapshot
        operationId: createImageSnapshot
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: path
          name: uuid
          description: Microservice UUID
          required: true
          type: string
        responses:
          '201':
            description: Created
            schema:
              type: object
              properties:
                id:
                  type: string
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '404':
            description: Invalid Microservice UUID
          '500':
            description: Internal Server Error
      get:
        tags:
        - Diagnostics
        description: Download image snapshot
        operationId: downloadImageSnapshot
        produces:
        - application/gzip
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: path
          name: uuid
          description: Microservice UUID
          required: true
          type: string
        responses:
          '200':
            description: Success
            schema:
              type: file
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '404':
            description: Invalid Microservice UUID
          '500':
            description: Internal Server Error
    '/microservices/{uuid}/strace':
      patch:
        tags:
        - Diagnostics
        description: Enables Microservice Strace Option
        operationId: enableMicroserviceStrace
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: path
          name: uuid
          description: Microservice UUID
          required: true
          type: string
        - in: body
          name: EnableOption
          description: Strace info to enable or disable feature
          required: true
          schema:
            type: object
            properties:
              enable:
                type: boolean
        responses:
          '204':
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Bad Request
          '401':
            description: Not Authorized
          '404':
            description: Invalid Microservice UUID
          '500':
            description: Internal Server Error
      get:
        tags:
        - Diagnostics
        description: Gets Strace Data for Microservice
        operationId: getMicroserviceStrace
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: path
          name: uuid
          description: Microservice UUID
          required: true
          type: string
        - in: query
          name: format
          required: true
          type: string
          enum:
          - file
          - string
        responses:
          '200':
            description: Success
            schema:
              type: object
              properties:
                data:
                  type: string
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '404':
            description: Invalid Microservice UUID
          '500':
            description: Internal Server Error
      put:
        tags:
        - Diagnostics
        description: Posts Microservice Strace file to FTP
        operationId: postMicroserviceStraceToFTP
        parameters:
        - in: path
          required: true
          name: uuid
          description: Microservice UUID
          type: string
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: body
          name: straceData
          required: true
          schema:
            $ref: '#/definitions/MicroserviceStraceFTPBody'
        responses:
          '204':
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Bad Request
          '401':
            description: Not Authorized
          '404':
            description: Invalid Microservice UUID
          '500':
            description: Internal Server Error
    '/iofog/{uuid}/tunnel':
      patch:
        tags:
        - Tunnel
        description: Opens/closes ssh tunnel
        operationId: openIOFogNodeSshTunnel
        parameters:
        - in: path
          name: uuid
          description: ioFog node id
          required: true
          type: string
        - in: body
          name: Action
          required: true
          schema:
            $ref: '#/definitions/ActionBody'
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        responses:
          '204':
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Bad Request
          '401':
            description: Not Authorized
          '404':
            description: Invalid Node Id
          '500':
            description: Internal Server Error
      get:
        tags:
        - Tunnel
        description: Gets current info about ioFog node ssh tunnel status
        operationId: getIOFogNodeSshTunnelStatusInfo
        parameters:
        - in: path
          name: uuid
          description: ioFog node id
          required: true
          type: string
        - in: header
          name: Authorization
          description: User token
          required: true
          type: string
        responses:
          '200':
            description: Success
            schema:
              $ref: '#/definitions/IOFogNodeTunnelStatusInfoResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '404':
            description: Invalid Node Id
          '500':
            description: Internal Server Error
    '/registries':
      post:
        tags:
        - Registries
        description: Creates new registry
        operationId: createRegistry
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: body
          name: New Registry
          required: false
          schema:
            $ref: '#/definitions/RegistryBody'
        responses:
          '201':
            description: Created
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Bad Request
          '401':
            description: Not Authorized
          '500':
            description: Internal Server Error
      get:
        tags:
        - Registries
        description: Gets list of registries
        operationId: getRegistryList
        parameters:
        - in: header
          name: Authorization
          description: User token
          required: true
          type: string
        responses:
          '200':
            description: Success
            schema:
              $ref: '#/definitions/RegistriesListResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '500':
            description: Internal Server Error
    '/registries/{id}':
      delete:
        tags:
        - Registries
        description: Deletes a registry
        operationId: deleteRegistry
        parameters:
        - in: path
          name: id
          description: Registry id
          required: true
          type: string
        - in: header
          name: Authorization
          description: User token
          required: true
          type: string
        responses:
          '204':
            description: Deleted
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '401':
            description: Not Authorized
          '404':
            description: Invalid Registry Id
          '500':
            description: Internal Server Error
      patch:
        tags:
        - Registries
        description: Updates a registry
        operationId: updateRegistry
        parameters:
        - in: path
          name: id
          description: Registry id
          required: true
          type: string
        - in: header
          name: Authorization
          description: User token
          required: true
          type: string
        - in: body
          name: Updates
          required: true
          schema:
            $ref: '#/definitions/RegistryBody'
        responses:
          '204':
            description: Updated
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          '400':
            description: Bad Request
          '401':
            description: Not Authorized
          '404':
            description: Invalid Registry Id
          '500':
            description: Internal Server Error
    '/user/login':
      post:
        tags:
        - User
        description: Login
        operationId: login
        parameters:
        - in: body
          name: credentials
          required: true
          schema:
            $ref: '#/definitions/LoginRequest'
        responses:
          200:
            description: Success
            schema:
              $ref: '#/definitions/LoginSuccessResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          400:
            description: bad request
          401:
            description: incorrect credentials
    '/user/logout':
      post:
        tags:
        - User
        description: Logout
        operationId: logout
        parameters:
        - in: header
          name: Authorization
          description: User token
          required: true
          type: string
        responses:
          204:
            description: Success
          401:
            description: Not Authorized
          500:
            description: Internal Server Error
    '/user/signup':
      post:
        tags:
        - User
        description: Signup
        operationId: signup
        parameters:
        - in: body
          name: user
          description: new user data
          required: true
          schema:
            $ref: '#/definitions/SignupRequest'
        responses:
          201:
            description: Success
            schema:
              $ref: '#/definitions/SignupSuccessResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          400:
            description: Bad Request
          500:
            description: Internal Server Error
    '/user/signup/resend-activation':
      get:
        tags:
        - User
        description: Resend activation email
        operationId: resendActivationEmail
        parameters:
        - in: query
          name: email
          required: true
          type: string
        responses:
          204:
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          400:
            description: Bad Request
          500:
            description: Internal Server Error
    '/user/activate':
      post:
        tags:
        - User
        description: Activate account
        operationId: activateAccount
        parameters:
        - in: body
          name: activationCode
          description: activation code
          required: true
          schema:
            $ref: '#/definitions/UserActivateRequest'
        responses:
          303:
            description: Redirect to login page
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
              Location:
                description: Login page url
                type: string
          404:
            description: Not Found
          500:
            description: Internal Server Error
    '/user/profile':
      get:
        tags:
        - User
        description: Get current user profile data
        operationId: getUserProfile
        parameters:
        - in: header
          name: Authorization
          description: User token
          required: true
          type: string
        responses:
          200:
            description: Success
            schema:
              $ref: '#/definitions/UserProfileDetailsResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          401:
            description: Not Authorized
          500:
            description: Internal Server Error
      patch:
        tags:
        - User
        description: Update user profile
        operationId: updateUserProfile
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: body
          name: profileData
          description: Updated profile data
          required: true
          schema:
            $ref: '#/definitions/UserProfileUpdatesRequest'
        responses:
          200:
            description: Updated user profile
            schema:
              $ref: '#/definitions/UserProfileDetailsResponse'
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          400:
            description: Bad Request
          401:
            description: Not Authorized
          500:
            description: Internal Server Error
      delete:
        tags:
        - User
        description: Delete account
        operationId: deleteAccount
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: body
          name: deleteParameters
          description: parameters for delete
          required: false
          schema:
            $ref: '#/definitions/DeleteParameters'
        responses:
          204:
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          401:
            description: Not Authorized
          500:
            description: Internal Server Error
    '/user/password':
      patch:
        tags:
        - User
        description: change password
        operationId: changePassword
        parameters:
        - in: header
          name: Authorization
          description: User Token
          required: true
          type: string
        - in: body
          name: passwordUpdates
          description: current and new password
          required: true
          schema:
            $ref: '#/definitions/PasswordChangeRequest'
        responses:
          204:
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          401:
            description: Not Authorized
          400:
            description: Bad Request
          500:
            description: Internal Server Error
      delete:
        tags:
        - User
        description: Reset password
        operationId: resetPassword
        parameters:
        - in: body
          name: email
          description: email
          required: true
          schema:
            $ref: '#/definitions/PasswordResetRequest'
        responses:
          204:
            description: Success
            headers:
              X-Timestamp:
                type: number
                description: FogController server timestamp
          404:
            description: Not Found
          500:
            description: Internal Server Error
  definitions:
    ServiceStatusResponse:
      type: object
      properties:
        status:
          type: string
          example: ok
        timestamp:
          type: number
    EmailActivationStatusResponse:
      type: object
      properties:
        isEmailActivationEnabled:
          type: boolean
    IOFogTypesResponse:
      type: object
      properties:
        fogTypes:
          type: array
          items:
            $ref: '#/definitions/IOFogType'
    IOFogType:
      type: object
      properties:
        id:
          type: number
        name:
          type: string
        image:
          type: string
        description:
          type: string
    IOFogNodesListFilters:
      type: array
      items:
        type: object
        properties:
          key:
            type: string
          value:
            type: string
          condition:
            type: string
    IOFogNodesListResponse:
      type: object
      properties:
        fogs:
          type: array
          items:
            $ref: '#/definitions/IOFogNodeInfoResponse'
    IOFogNodeInfoResponse:
      type: object
      properties:
        id:
          type: string
        name:
          type: string
        location:
          type: string
        gpsMode:
          type: string
        latitude:
          type: number
        longitude:
          type: number
        description:
          type: string
        createAt:
          type: number
        updatedAt:
          type: number
        lastActive:
          type: number
        daemonStatus:
          type: string
        daemonOperatingDuration:
          type: number
        daemonLastStart:
          type: number
        memoryUsage:
          type: number
        diskUsage:
          type: number
        cpuUsage:
          type: number
        memoryViolation:
          type: boolean
        diskViolation:
          type: boolean
        cpuViolation:
          type: boolean
        systemAvailableDisk:
          type: integer
        systemAvailableMemory:
           type: integer
        systemTotalCpu:
           type: number
        securityStatus:
           type: string
        securityViolationInfo:
           type: string
        microserviceStatus:
          type: string
        repositoryCount:
          type: number
        repositoryStatus:
          type: string
        lastStatusTime:
          type: number
        ipAddress:
          type: string
        processedMessages:
          type: number
        microserviceMessageCounts:
          type: number
        messageSpeed:
          type: number
        lastCommandTime:
          type: number
        networkInterface:
          type: string
        dockerUrl:
          type: string
        diskLimit:
          type: number
        diskDirectory:
          type: string
        memoryLimit:
          type: number
        cpuLimit:
          type: number
        logLimit:
          type: number
        logDirectory:
          type: string
        bluetoothEnabled:
          type: boolean
        abstractedHardwareEnabled:
          type: boolean
        watchdogEnabled:
          type: boolean
        logFileCount:
          type: number
        version:
          type: string
        isReadyToUpgrade:
          type: boolean
        isReadyToRollback:
          type: boolean
        statusFrequency:
          type: number
        changeFrequency:
          type: number
        deviceScanFrequency:
          type: number
        tunnel:
          type: string
        type:
          $ref: '#/definitions/IOFogType'
    UpdateIOFogNodeRequestBody:
      type: object
      properties:
        name:
          type: string
        location:
          type: string
        latitude:
          type: number
        longitude:
          type: number
        description:
          type: string
        dockerUrl:
          type: string
          default: 'unix:///var/run/docker.sock'
        diskLimit:
          type: number
          default: 50
        diskDirectory:
          type: string
          default: /var/lib/iofog
        memoryLimit:
          type: number
          default: 4096
          format: MB
        cpuLimit:
          type: number
          default: 80
          format: percent
        logLimit:
          type: number
          default: 10
          format: GB
        logDirectory:
          type: string
          default: /var/log/iofog
        logFileCount:
          type: number
          default: 10
        statusFrequency:
          type: number
          default: 10
          format: seconds
        changeFrequency:
          type: number
          default: 20
          format: seconds
        deviceScanFrequency:
          type: number
          default: 20
          format: seconds
        bluetoothEnabled:
          type: boolean
          default: false
        watchdogEnabled:
          type: boolean
          default: true
        abstractedHardwareEnabled:
          type: boolean
          default: false
        fogType:
          type: number
    NewIOFogNodeResponse:
      type: object
      properties:
        id:
          type: string
    ProvisioningKeyResponse:
      type: object
      properties:
        key:
          type: string
        expirationTime:
          type: number
    AgentProvisioningRequest:
      type: object
      required:
      - type
      - key
      properties:
        type:
          type: number
          enum:
          - 1
          - 2
          description: >
            Architecture
             * '1': x86
             * '2': arm
        key:
          type: string
          description: provisioning key
    AgentProvisioningResponse:
      type: object
      properties:
        id:
          type: string
          description: ioFog Node Id
        token:
          type: string
          description: Agent token
    AgentDeprovisioningRequest:
      type: object
      properties:
        microserviceUuids:
          type: array
          items:
            type: string
    IOFogNodeConfigChanges:
      type: object
      properties:
        config:
          type: boolean
        version:
          type: boolean
        reboot:
          type: boolean
        deleteNode:
          type: boolean
        microservicesList:
          type: boolean
        microservicesConfig:
          type: boolean
        routing:
          type: boolean
        registries:
          type: boolean
        tunnel:
          type: boolean
        diagnostics:
          type: boolean
        isImageSnapshot:
          type: boolean
    IOFogNodeConfig:
      type: object
      properties:
        networkInterface:
          type: string
        dockerUrl:
          type: string
        diskLimit:
          type: number
        diskDirectory:
          type: string
        memoryLimit:
          type: number
        cpuLimit:
          type: number
        logLimit:
          type: number
        logDirectory:
          type: string
        logFileCount:
          type: number
        statusFrequency:
          type: number
        changeFrequency:
          type: number
        deviceScanFrequency:
          type: number
        watchdogEnabled:
          type: boolean
        latitude:
          type: number
        longitude:
          type: number
    IOFogNodeConfigRequest:
      type: object
      properties:
        networkInterface:
          type: string
        dockerUrl:
          type: string
        diskLimit:
          type: number
        diskDirectory:
          type: string
        memoryLimit:
          type: number
        cpuLimit:
          type: number
        logLimit:
          type: number
        logDirectory:
          type: string
        logFileCount:
          type: number
        statusFrequency:
          type: number
        changeFrequency:
          type: number
        deviceScanFrequency:
          type: number
        watchdogEnabled:
          type: boolean
        latitude:
          type: number
        longitude:
          type: number
        gpsMode:
          type: string
    AgentStatus:
      type: object
      properties:
        daemonStatus:
          type: string
        daemonOperatingDuration:
          type: number
        daemonLastStart:
          type: number
        memoryUsage:
          type: number
        diskUsage:
          type: number
        cpuUsage:
          type: number
        memoryViolation:
          type: boolean
        diskViolation:
          type: boolean
        cpuViolation:
          type: boolean
        systemAvailableDisk:
          type: integer
        systemAvailableMemory:
          type: integer
        systemTotalCpu:
          type: number
        securityStatus:
          type: string
        securityViolationInfo:
          type: string
        microserviceStatus:
          type: string
        repositoryCount:
          type: number
        repositoryStatus:
          type: string
        systemTime:
          type: number
        lastStatusTime:
          type: number
        ipAddress:
          type: string
        processedMessages:
          type: number
        microserviceMessageCounts:
          type: string
        messageSpeed:
          type: number
        lastCommandTime:
          type: number
        tunnelStatus:
          type: string
        version:
          type: string
        isReadyToUpgrade:
          type: boolean
        isReadyToRollback:
          type: boolean
    IOFogNodeTunnelConfigResponse:
      type: object
      properties:
        username:
          type: string
        password:
          type: string
        host:
          type: string
        remotePort:
          type: number
        localPort:
          type: number
        rsaKey:
          type: string
        closed:
          type: boolean
    IOFogNodeStraceResponse:
      type: object
      properties:
        straceValues:
          type: array
          items:
            $ref: '#/definitions/MicroserviceStrace'
    MicroserviceStrace:
      type: object
      properties:
        microserviceUuid:
          type: string
        straceRun:
          type: boolean
    IOFogNodeStraceBuffer:
      type: object
      properties:
        straceData:
          type: array
          items:
            $ref: '#/definitions/MicroserviceStraceBuffer'
    MicroserviceStraceBuffer:
      type: object
      properties:
        microserviceUuid:
          type: string
        buffer:
          type: string
    MicroserviceStraceFTPBody:
      type: object
      properties:
        ftpHost:
          type: string
        ftpPort:
          type: number
        ftpUser:
          type: string
        ftpPass:
          type: string
        ftpDestDir:
          type: string
    AgentMicroservicesListResponse:
      type: object
      properties:
        microservices:
          type: array
          items:
            $ref: '#/definitions/AgentMicroservicesInfoResponse'
    AgentMicroservicesInfoResponse:
      type: object
      properties:
        uuid:
          type: string
        needUpdate:
          type: boolean
        rebuild:
          type: boolean
        rootHostAccess:
          type: boolean
        logSize:
          type: number
        imageId:
          type: string
        registryUrl:
          type: string
        portMappings:
          type: array
          items:
            $ref: '#/definitions/PortMappingAgentRequest'
        VolumeMappings:
          type: array
          items:
            $ref: '#/definitions/VolumeMapping'
        imageSnapshot:
          type: string
        removeWithCleanUp:
          type: boolean
        routes:
          $ref: '#/definitions/ReceiverMicroservices'
    ReceiverMicroservices:
      type: array
      items:
        type: string
    VolumeMapping:
      type: object
      properties:
        hostDestination:
          type: string
          example: /var/dest
        containerDestination:
          type: string
          example: /var/dest
        accessMode:
          type: string
          example: rw
    VolumeMappingResponse:
      type: object
      properties:
        volumeMappings:
          type: array
          items:
            $ref: '#/definitions/VolumeMappingRequest'
    VolumeMappingRequest:
      type: object
      properties:
        id:
          type: number
        hostDestination:
          type: string
          example: /var/dest
        containerDestination:
          type: string
          example: /var/dest
        accessMode:
          type: string
          example: rw
    PortMappingsResponse:
      type: object
      properties:
        internal:
          type: number
        external:
          type: number
        publicMode:
          type: boolean
    PortMappingsRequest:
      type: object
      properties:
        internal:
          type: number
        external:
          type: number
        publicMode:
          type: boolean
    PortMappingsPublicResponse:
      type: object
      properties:
        publicIp:
          type: string
        publicPort:
          type: number
    PortMappingsListResponse:
      type: object
      properties:
        ports:
          type: array
          items:
            $ref: '#/definitions/PortMappingsItemResponse'
    PortMappingsItemResponse:
      type: object
      properties:
        internal:
          type: number
        external:
          type: number
        publicMode:
          type: boolean
        publicIp:
          type: string
        publicPort:
          type: number
    PortMappingAgentRequest:
      type: object
      properties:
        outsidecontainer:
          type: string
          example: 80
        insidecontainer:
          type: string
          example: 80
    RegistriesListResponse:
      type: object
      properties:
        registries:
          type: array
          items:
            $ref: '#/definitions/RegistryResponse'
    RegistryResponse:
      type: object
      properties:
        id:
          type: number
        url:
          type: string
        isPublic:
          type: boolean
        isSecure:
          type: boolean
        certificate:
          type: string
        requiresCert:
          type: boolean
        username:
          type: string
        password:
          type: string
        userEmail:
          type: string
        userId:
          type: string
    RegistryBody:
      type: object
      properties:
        url:
          type: string
        isPublic:
          type: boolean
        username:
          type: string
        password:
          type: string
        email:
          type: string
        requiresCert:
          type: boolean
        certificate:
          type: string
    ActionBody:
      type: object
      properties:
        action:
          type: string
          enum:
          - open
          - close
    MicroservicesCatalogResponse:
      type: object
      properties:
        catalogItems:
          type: array
          items:
            $ref: '#/definitions/CatalogItemInfoResponse'
    CatalogItemInfoResponse:
      type: object
      properties:
        id:
          type: string
        name:
          type: string
        description:
          type: string
        category:
          type: string
        publisher:
          type: string
        diskRequired:
          type: number
        ramRequired:
          type: number
        picture:
          type: string
        isPublic:
          type: boolean
        registryId:
          type: number
        inputType:
          $ref: '#/definitions/InfoTypeResponse'
        outputType:
          $ref: '#/definitions/InfoTypeResponse'
        configExample:
          type: string
        images:
          $ref: '#/definitions/MicroserviceContainerImages'
    InfoTypeResponse:
      type: object
      properties:
        infoType:
          type: string
        infoFormat:
          type: string
    MicroserviceContainerImages:
      type: array
      items:
        $ref: '#/definitions/MicroserviceContainerImage'
    MicroserviceContainerImage:
      type: object
      properties:
        containerImage:
          type: string
        fogTypeId:
          type: number
          enum:
          - 1
          - 2
    CreateUpdateCatalogItemRequestBody:
      type: object
      properties:
        name:
          type: string
        description:
          type: string
        category:
          type: string
        images:
          $ref: '#/definitions/MicroserviceContainerImages'
        publisher:
          type: string
        diskRequired:
          type: number
        ramRequired:
          type: number
        picture:
          type: string
        isPublic:
          type: boolean
        registryId:
          type: number
        inputType:
          $ref: '#/definitions/InfoTypeResponse'
        outputType:
          $ref: '#/definitions/InfoTypeResponse'
        configExample:
          type: string
    GetFlowsResponse:
      type: object
      properties:
        flows:
          type: array
          items:
            $ref: '#/definitions/FlowInfoResponse'
    FlowInfoResponse:
      type: object
      properties:
        id:
          type: string
        name:
          type: string
        description:
          type: string
        isActivated:
          type: boolean
    NewFlowRequest:
      type: object
      properties:
        name:
          type: string
        description:
          type: string
        isActivated:
          type: boolean
    GetMicroservicesResponse:
      type: object
      properties:
        microservices:
          type: array
          items:
            $ref: '#/definitions/MicroserviceInfoResponse'
    MicroserviceInfoResponse:
      type: object
      properties:
        uuid:
          type: string
        name:
          type: string
        config:
          type: string
        rootHostAccess:
          type: boolean
        logLimit:
          type: number
        volumeMappings:
          type: array
          items:
            $ref: '#/definitions/VolumeMappingRequest'
        ports:
          type: array
          items:
            $ref: '#/definitions/PortMappingsResponse'
        routes:
          $ref: '#/definitions/ReceiverMicroservices'
    NewMicroserviceRequest:
      type: object
      properties:
        name:
          type: string
        config:
          type: string
        catalogItemId:
          type: integer
        flowId:
          type: integer
        iofogUuid:
          type: string
        rootHostAccess:
          type: boolean
        logSize:
          type: number
        volumeMappings:
          type: array
          items:
            $ref: '#/definitions/VolumeMapping'
        ports:
          type: array
          items:
            $ref: '#/definitions/PortMappingsRequest'
        routes:
          $ref: '#/definitions/ReceiverMicroservices'
    UpdateMicroserviceRequest:
      type: object
      required:
      - name
      properties:
        name:
          type: string
        config:
          type: string
        rebuild:
          type: boolean
        iofogUuid:
          type: string
        rootHostAccess:
          type: boolean
        logLimit:
          type: number
        volumeMappings:
          type: array
          items:
            $ref: '#/definitions/VolumeMapping'
    IOFogNodeTunnelStatusInfoResponse:
      type: object
      properties:
        username:
          type: string
        host:
          type: string
        remotePort:
          type: number
        localPort:
          type: number
        status:
          type: string
    LoginRequest:
      type: object
      required:
      - email
      - password
      properties:
        email:
          type: string
        password:
          type: string
    LoginSuccessResponse:
      type: object
      required:
      - accessToken
      properties:
        accessToken:
          type: string
    SignupSuccessResponse:
      type: object
      required:
      - userId
      - firstName
      - lastName
      - email
      - emailActivated
      properties:
        userId:
          type: number
        firstName:
          type: string
        lastName:
          type: string
        email:
          type: string
        emailActivated:
          type: boolean
    UserProfileDetailsResponse:
      type: object
      required:
      - firstName
      - lastName
      - email
      properties:
        firstName:
          type: string
        lastName:
          type: string
        email:
          type: string
    UserProfileUpdatesRequest:
      type: object
      properties:
        firstName:
          type: string
        lastName:
          type: string
    UserActivateRequest:
      type: object
      required:
      - activationCode
      properties:
        activationCode:
          type: string
    PasswordResetRequest:
      type: object
      required:
      - email
      properties:
        email:
          type: string
    PasswordChangeRequest:
      type: object
      required:
      - oldPassword
      - newPassword
      properties:
        oldPassword:
          type: string
        newPassword:
          type: string
    DeleteParameters:
      type: object
      required:
      - force
      properties:
        force:
          type: boolean
    SignupRequest:
      type: object
      required:
      - firstName
      - lastName
      - email
      - password
      properties:
        firstName:
          type: string
        lastName:
          type: string
        email:
          type: string
        password:
          type: string
    VersionCommandResponse:
      type: object
      required:
      - versionCommand
      - provisionKey
      - expirationTime
      properties:
        versionCommand:
          type: string
        provisionKey:
          type: string
        expirationTime:
          type: string
    HalInfo:
      type: object
      required:
      - info
      properties:
        info:
          type: string
    ImageSnapshotResponse:
      type: object
      required:
      - uuid
      properties:
        uuid:
          type: string
    ImageSnapshotRequest:
      type: object
      required:
      - upstream
      properties:
        upstream:
          type: string
    PostTrackingRequest: 
      type: array
      items:
        $ref: '#/definitions/TrackingEvent'
    TrackingEvent:
      type: object
      required:
      - uuid
      properties:
        uuid:
          type: string
        sourceType:
          type: string
        timestamp:
          type: number
        type:
          type: string
        data: 
          type: object
  schemes:
  - http
  - https
  host: 'localhost:54421'
  basePath: /api/v3
</swagger-ui>
