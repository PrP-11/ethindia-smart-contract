# ethindia-smart-contract

pragma solidity ^0.4.17;

contract authorContract {

	// current blog id increments on every upload object
	uint currentId = 1;
	uint grantId = 1;

	struct UploadObject {
		// this is the public address of the author
		address authorAddress; 
		// file hash that contains encypted data
		string ciphertext;
		// capsule id
		string capsuleId;
	}

	struct GrantObject {
		// proxy re-encryption related data
		string policyId;
		string capsuleId;
		string signedPublicKey;
		string pubKey;
	}

	// user and their upload id
	// mapping (address => uint[]) public addressUploadID;

	// map between the id and its data
	mapping (uint => UploadObject) public idUploadObject;
	mapping (uint => GrantObject) public idGrantObject;

	// add new entry
	function addArticle(address _from, string ciphertext, string capsuleId) public returns(uint) {
		idUploadObject[currentId].authorAddress = _from;
		idUploadObject[currentId].ciphertext = ciphertext;
		idUploadObject[currentId].capsuleId = capsuleId;

		currentId = currentId + 1;
		return currentId;
	}

	function addGrant(string policyId, string capsuleId, string signedPublicKey, string pubKey) public returns(uint) {
		idGrantObject[grantId].policyId = policyId;
		idGrantObject[grantId].capsuleId = capsuleId;
		idGrantObject[grantId].signedPublicKey = signedPublicKey;
		idGrantObject[grantId].pubKey = pubKey;

		grantId = grantId + 1;
		return grantId;
	}

	// get existing documents for an address
	function getArticle(string capsuleId) public view returns(address, string, string) {
		for (uint index = 1; index <= currentId; index++) {
			if(keccak256(idUploadObject[index].capsuleId) == keccak256(capsuleId)) {
				return (idUploadObject[index].authorAddress, idUploadObject[index].ciphertext, idUploadObject[index].capsuleId);
			}
		}
	}

	function getGrant(string policyId) public view returns(string, string, string, string) {
		for (uint index = 1; index <= currentId; index++) {
			if(keccak256(idGrantObject[index].policyId) == keccak256(policyId)) {
				return (idGrantObject[index].policyId, idGrantObject[index].capsuleId, idGrantObject[index].signedPublicKey, idGrantObject[index].signedPublicKey);
			}
		}
	}

	function getCurrentId() public view returns(uint) {
		return currentId;
	}

	function getGrantId() public view returns(uint) {
		return grantId;
	}
}
