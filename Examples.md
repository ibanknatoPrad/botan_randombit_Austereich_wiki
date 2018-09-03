The source code for the command line interface (https://github.com/randombit/botan/tree/master/src/cli) is intended to demonstrate usage of many APIs, take a look.

Other usage examples follow, feel free to add more.

# AEAD Encryption

```
#include <botan/aead.h>
#include <botan/system_rng.h>
#include <botan/hex.h>
#include <iostream>

int main()                                                                                                                                                                                     
   {                                                                                                                                                                                           
   using namespace Botan;                                                                                                                                                                      
                                                                                                                                                                                               
   const std::string chosen_aead_mode = "ChaCha20Poly1305";                                                                                                                                    
                                                                                                                                                                                               
   // a key from somewhere                                                                                                                                                                     
   auto key = system_rng().random_vec(32);                                                                                                                                                     
                                                                                                                                                                                               
   // data from somewhere ...                                                                                                                                                                  
   const uint8_t ptext[32] = { 0 };                                                                                                                                                            
   const size_t ptext_len = sizeof(ptext);                                                                                                                                                     
   secure_vector<uint8_t> buf(ptext, ptext + ptext_len);                                                                                                                                       
                                                                                                                                                                                               
   // create the aead object                                                                                                                                                                   
   std::unique_ptr<AEAD_Mode> aead = AEAD_Mode::create_or_throw(chosen_aead_mode, ENCRYPTION);                                                                                                 
                                                                                                                                                                                               
   // set key                                                                                                                                                                                  
   aead->set_key(key);                                                                                                                                                                         
                                                                                                                                                                                               
   // chose a random nonce of whatever length aead wants                                                                                                                                       
   auto nonce = system_rng().random_vec(aead->default_nonce_length());                                                                                                                         
                                                                                                                                                                                               
   // begin processing using nonce                                                                                                                                                             
   aead->start(nonce);                                                                                                                                                                         
                                                                                                                                                                                               
   // process the entire message in buf in one go, appending tag                                                                                                                               
   aead->finish(buf);                                                                                                                                                                          
                                                                                                                                                                                               
   // output nonce and ciphertext                                                                                                                                                              
   std::cout << hex_encode(nonce) << hex_encode(buf) << "\n";                                                                                                                                  
   }
```