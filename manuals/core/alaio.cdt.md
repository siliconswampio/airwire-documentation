# ALAIO.CDT (Contract Development Toolkit)
## Version : 1.7.0

ALAIO.CDT is a toolchain for WebAssembly (WASM) and set of tools to facilitate smart contract development for the ALAIO platform. In addition to being a general purpose WebAssembly toolchain, [ALAIO]() specific optimizations are available to support building ALAIO smart contracts. This new toolchain is built around [Clang 7](), which means that ALAIO.CDT has the most currently available optimizations and analyses from LLVM, but as the WASM target is still considered experimental, some optimizations are incomplete or not available.

## New Introductions

As of this release two new repositories are under the suite of tools provided by **ALAIO.CDT**. These are the [Ricardian Template Toolkit]() and the [Ricardian Specification](). The **Ricardian Template Toolkit** is a set of libraries to assist smart contract developers in craftinng their Ricardian contracts. The Ricardian specification is the working specification for the above mentioned toolkit. Please note that both projects are **alpha** releases and are subject to change.

## Upgrading

There's been a round of breaking changes, if you are upgrading please read the [Upgrade guide from 1.2 to 1.3]() and [Upgrade guide from 1.5 to 1.6]().

## Contributing

[Contributing Guide]()

[Code of Conduct]()

## License

[MIT]()

## Important

See LICENSE for copyright and license terms. Alacrity makes its contribution on a voluntary basis as a member of the ALAIO community and is not responsible for ensuring the overall performance of the software or any related applications. We make no representation, warranty, guarantee or undertaking in respect of the software or any related documentation, whether expressed or implied, including but not limited to the warranties or merchantability, fitness for a particular purpose and noninfringement. In no event shall we be liable for any claim, damages or other liability, whether in an action of contract, tort or otherwise, arising from, out of or in connection with the software or documentation or the use or other dealings in the software or documentation. Any test results or performance figures are indicative and will not reflect performance under all conditions. Any reference to any third party or third-party product, service or other resource is not an endorsement or recommendation by Alacrity. We are not responsible, and disclaim any and all responsibility and liability, for your use of or reliance on any of these resources. Third-party resources may be updated, changed or terminated at any time, so the information here may be out of date or inaccurate.