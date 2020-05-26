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
            while (Test(10) == false)
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
#Class EncryptorDecryptor
class EncryptorDecryptor
    {
        public BigInteger c, m;

        public void Encrypt(BigInteger m, BigInteger e, BigInteger n)          //encryption
        {
            c = BigInteger.ModPow(m, e, n);
        }

        public void Decrypt(BigInteger cResult, BigInteger d, BigInteger n)    //decryption
        {
            m = BigInteger.ModPow(cResult, d, n);
        }
    }
#KeyGenerator Encryption Code
  KeyGenerator p = new KeyGenerator();
                KeyGenerator q = new KeyGenerator();
                KeyGenerator e = new KeyGenerator();
                KeyGenerator f = new KeyGenerator();
                KeyGenerator pub_key = new KeyGenerator();
                KeyGenerator priv_key = new KeyGenerator();
                EncryptorDecryptor cipher = new EncryptorDecryptor();
                EncryptorDecryptor plain = new EncryptorDecryptor();

                p.randomGenerator();
                q.randomGenerator();
                e.randomGenerator();
                
                BigInteger n;
                if (p.Test(10) == true && q.Test(10) == true)
                {
                    n = p.result * q.result;
                }
                else
                {
                    p.GetNearestPrime();
                    q.GetNearestPrime();
                    n = p.result * q.result;
                }

                f.EulerFunction(p.result , q.result);
                pub_key.GeneratePublicKey(e.result, n);
                priv_key.GeneratePrivateKey(e.result);

                Console.WriteLine("Enter your message:");
                string msg = Console.ReadLine(); 
                BigInteger m = BigInteger.Parse(msg);

                cipher.Encrypt(m, e.result, n);
                Console.WriteLine("The ciphertext is : {0}", cipher.c);

                plain.Decrypt(cipher.c, priv_key.d, n);
                Console.WriteLine("The plaintext is : {0}", plain.m);

            }
        }
    }

# PROJECT REPORT:

Name Of Project:
RSA Cryptography

Language:
C#(sharp)

Class Id: 103348

Group Members:
HASSAN KHAN (group leader) 64089
Mohammad Humayun Khalid 64086
Muhammad Hashir 64093
Muhammad Saim 64088

Project Description
This project is all about Cryptography in which you may get knowledge about
Cryptography and some points are written below:
The RSA cryptosystem is the most widely-used public key cryptography algorithm
in the world. This is the original algorithm:
Generate two large random primes, p and q, of approximately equal size such
that their product n = pq is of the required bit length, e.g. 1024 bits.
Compute n = pq and (φ) φ = (p-1)(q-1).
Choose an integer e, 1 < e < φ, such that gcd(e, φ) = 1.
Compute the secret exponent d, 1 < d < phi, such that ed ≡ 1 (mod phi).
The public key is (n, e) and the private key (d, p, q). Keep all the values d, p, q and
phi secret. [We prefer sometimes to write the private key as (n, d) because you
need the value of n when using d. Other times we might write the key pair as ((N,
e), d).]
About the variables:
p,q are the most common variables for prime numbers.
n is known as the modulus.
