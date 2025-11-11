
https://www.reddit.com/r/mcp/s/VptZlOivdR

Hi there,

  
As I like developing in front-end tech such as Angular, I wanted to experiment with MCP servers and clients using my favorite stack.  And also managing the interactions via the UI of the front end directly, instead of on a node-server.

To my delight it is actually very easy getting that done by using a custom Transport that just passes the RPC through function. 

I called it memory-transport, easily created out of the Transport spec and happy to share here.

I am using a Telegram bot for the real UI - and the webapp is just for managing the MCP stuff (and later stage agentic work)

Purely for developer delight ! :)

Kudos to the community - loving what you all do!





    import { Transport } from '@modelcontextprotocol/sdk/shared/transport';
    import { JSONRPCMessage } from '@modelcontextprotocol/sdk/types';
    
    /**
     * Transport implementation that uses memory to pass messages between two endpoints
     * within the same application. Useful for testing or for scenarios where both
     * client and server are running in the same process.
     */
    export class MemoryTransport implements Transport {
      private _partner: MemoryTransport | null = null;
      sessionId: string;
      debug: boolean = false;
    
      onclose?: () => void;
      onerror?: (error: Error) => void;
      onmessage?: (message: JSONRPCMessage) => void;
    
      /**
       * Creates a new MemoryTransport instance
       * @param partner Optional partner transport to connect to
       * @param debug Optional flag to enable debug logging
       */
      constructor(partner?: MemoryTransport | null, debug?: boolean) {
        this.sessionId = crypto.randomUUID();
        if (partner) {
          this._connect(partner);
        }
    
        if (debug) {
          this.debug = debug;
        }
      }
    
      start(): Promise<void> {
        return Promise.resolve();
      }
    
      /**
       * Connects this transport instance to another MemoryTransport instance.
       * Messages sent from this instance will be received by the partner and vice versa.
       *
       * @param partner The MemoryTransport instance to connect to
       */
      _connect(partner: MemoryTransport): void {
        if (this._partner) {
          throw new Error('This transport is already connected to a partner');
        }
        if (partner._partner) {
          throw new Error(
            'Partner transport is already connected to another transport'
          );
        }
    
        this._partner = partner;
        partner._partner = this;
      }
    
      /**
       * Sends a JSON-RPC message to the connected partner transport.
       *
       * @param message The JSON-RPC message to send
       */
      async send(message: JSONRPCMessage): Promise<void> {
        if (!this._partner) {
          throw new Error('No partner transport connected');
        }
    
        // Create a copy of the message to prevent mutation issues
        const messageCopy = JSON.parse(JSON.stringify(message));
    
        if (this.debug)
          console.log(`Transport ${this.sessionId} sending message:`, messageCopy);
    
        // Use setTimeout to make this asynchronous, simulating network behavior
        setTimeout(() => {
          if (this._partner && this._partner.onmessage) {
            this._partner.onmessage(messageCopy);
          }
        }, 0);
    
        return Promise.resolve();
      }
    
      /**
       * Closes the connection to the partner transport.
       */
      async close(): Promise<void> {
        // Notify the partner about disconnection
        if (this._partner) {
          const partner = this._partner;
          this._partner = null;
    
          // Remove the reference to this transport from partner
          partner._partner = null;
    
          // Notify partner about connection closure
          setTimeout(() => {
            partner.onclose?.();
          }, 0);
        }
    
        // Notify this transport about connection closure
        this.onclose?.();
    
        return Promise.resolve();
      }
    }
    
    
    