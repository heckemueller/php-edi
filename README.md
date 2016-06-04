# php-edi
EDI transactions with SF3 / PHP 7

Simplify data mapping (in/out) for business document exchange.

Coming up soon (WIP).


#Implementation Scratch:
EDI
	- Reader
		public MessageDirectory\Interfaces\Reader\CanvasInterface read( stream, translationLevel [, Callback callback = null]? )

	- Writer
		public MessageDirectory\Interfaces\Writer\CanvasInterface getNewCanvas( $protocolName )
		public MessageDirectory\Interfaces\Writer\MessageInterface getNewMessage( $messageType, $messageVersion )

	- Gateway
		- AbstractGateway: Traversable(-Trait?)
			public observe( path, mask = null )
			public hasNewMessages()
			abstract protected registerNewMessage()

		- Filesystem
		- PDO Database
		- FTP
		- SFTP

	- MessageDirectory/
		- Interfaces/
			- Reader/
				- CanvasInterface
					public String getSender()
					public String getSenderQualifier()
					public String getReceiver()
					public String getReceiverQualifier()
					public Bool getIsTest()
					public String getReference()
					public \DateTime getDatetimeOfCreation()
					public MessageInterface[] getMessages()
					public null|String getMessageTypeOfDocumentsInCanvas()

				- MessageInterface
					public String getReference()
					public String getMessageType()
					public (DOMDocument|SimpleXMLElement|String) ? parseMessageAsXml()
					
			- Writer/
				- CanvasInterface
					public void setSender()
					public void setSenderQualifier()
					public void setReceiver()
					public void setReceiverQualifier()
					public void setIsTest()
					public void setReference()
					public void setDatetimeOfCreation()
					public void setMessageType()
					public Bool addMessage( MessageInterface $message, $translationLevel = (RAW_XML|SIMPLIFIED|READABLE) )
					public String parse()
					public Bool save( $message, GatewayInterface $gateway, $messagePathAndName )
					public Bool parseAndSave( GatewayInterface $gateway, $messagePathAndName )

				- MessageInterface
					public void setReference( )
					public void setMessageType( )
					public void setMessageData( $data )
					protected void validate( $mode = (STRICT|~STRICT) )
		- Impl/
			- Edifact/
				- Directory/
					- D93A/
						- Segments/
							C506
						ORDERS
						INVRPT
						DESADV
						RETANN
						RETINS [...]
					- D[...]
				- Reader/
					- Canvas
					- Message
				- Writer/
					- Canvas
					- Message

			- X12
				- Directory/
					- 850/
					....
				Reader/
					Canvas
					Message
				Writer/
					Canvas
					Message

			- Flatfile
				Reader/
					Canvas
					Message
				Writer/
					Canvas
					Message
