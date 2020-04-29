# Discrete-Mathematics-proposal
 
“PROJECT PROPOSAL OF DISCRETE MATHEMATICS” 
PROJECT NAME:
“RSA Cryptography”
SOFTWARE LANGUAGE:
“C#(C sharp)”
PROJECT DETAILS & FEATURES:
In This project we present you the encryption and decryption according to the coding in which encryption is done by public key and once a messaged has been done than it decrypt it by private key.
PRIVATE KEY DETAILS:
RSA key is a private key based on RSA algorithm. Private Key is used for authentication and a symmetric key.

ALGORITHM:
•	RSA algorithm is a popular exponentiation in a finite field over integers including prime numbers.
•	The integers used by this method are sufficiently large making it difficult to solve.
•	There are two sets of keys in this algorithm: private key and public key.

GROUP MEMBERS:
1.	LEADER:HASSAN KHAN(64089)
2.	MOHAMMAD HUMAYUN(64086)
3.	MOHAMMAD SAIM(64088)
4.	MUHAMMAD HASHIR(64093)


#Code 
#RSA 
# CLASS KEYGENERATOR
class KeyGenerator
    {
        public BigInteger result, d;
        public int f;
        
        public void randomGenerator()                            //create random number
        {
            RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider();
            byte[] randomNumber = new byte[64];
            rng.GetBytes(randomNumber);
            result = new BigInteger(randomNumber);
            result = BigInteger.Abs(result);
        }

           public bool Test(int k) 
        {
            if (result == 2 || result == 3)
                return true;
            if (result < 2 || result % 2 == 0)
                return false;

            BigInteger d = result - 1;
            int s = 0;

            while (d % 2 == 0)
            {
                d /= 2;
                s += 1;

            }

            for (int i = 0; i < k; i++)
            {
                RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider();
                byte[] _a = new byte[result.ToByteArray().LongLength];
                BigInteger a;

                do
                {
                    rng.GetBytes(_a);
                    a = new BigInteger(_a);
                }
                while (a < 2 || a >= result - 2);

                BigInteger x = BigInteger.ModPow(a, d, result);

                if (x == 1 || x == result - 1)
                    continue;

                for (int r = 1; r < s; r++)
                {
                    x = BigInteger.ModPow(x, 2, result);

                    if (x == 1)
                        return false;
                    if (x == result - 1)
                        break;
                }

                if (x != result - 1)
                    return false;
            }
            return true;
        }
        public BigInteger GetNearestPrime()                  //get the next prime
        {
            while (MillerRabinTest(10) == false)
            {
                result++;
            }
            return result;
        }
       
           public void EulerFunction(int p, int q)          //euler function: f(n) = (p-1)(q-1)
             {
                int m = p - 1;
                int n = q - 1;
                f = m * n;
           }
        

        public BigInteger GeneratePublicKey(BigInteger e, BigInteger n)        // public key
        {
            while (e > 1)
            {
                BigInteger ef = BigInteger.Pow(e, f);
                ef = 1 % n;
            }
            return e;
        }

        public void GeneratePrivateKey(BigInteger e)                  //private key
        {
            d = (1 / e) % f;
        }
    }
}
