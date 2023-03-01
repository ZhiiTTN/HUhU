# HUhU
ce shi 
:)
package main

import (
	"archive/tar"
	"compress/gzip"
	"io"
	"log"
	"os"
	"path"
)

func compressFile(filename string, output string) error {
	file, err := os.Create(output)
	if err != nil {
		return err
	}

	writer, err := gzip.NewWriterLevel(file, gzip.BestCompression)
	if err != nil {
		return err
	}

	tw := tar.NewWriter(writer)

	fileToCompress, err := os.Open(filename)
	if err != nil {
		return err
	}

	body, err := io.ReadAll(fileToCompress)
	if err != nil {
		return err
	}

	hdr := &tar.Header{
		Name: path.Base(filename),
		Mode: 0600,
		Size: int64(len(body)),
	}
	err = tw.WriteHeader(hdr)
	if err != nil {
		return err
	}

	_, err = tw.Write(body)
	if err != nil {
		return err
	}
	log.Print("Compressed " + filename + " to " + output)
	if err := tw.Close(); err != nil {
		return err
	}
	if err := writer.Close(); err != nil {
		return err
	}
	if err := file.Close(); err != nil {
		return err
	}
	if err := fileToCompress.Close(); err != nil {
		return err
	}
	return nil
}

func decompressFile(filename string, output string) error {
	file, err := os.Open(filename)
	if err != nil {
		return err
	}

	reader, err := gzip.NewReader(file)
	if err != nil {
		return err
	}

	tr := tar.NewReader(reader)

	hdr, err := tr.Next()
	if err != nil {
		return err
	}

	fileToDecompress, err := os.Create(output)
	if err != nil {
		return err
	}

	_, err = io.Copy(fileToDecompress, tr)
	if err != nil {
		return err
	}
	log.Print("Decompressed " + hdr.Name + " to " + output)
	if err := fileToDecompress.Close(); err != nil {
		return err
	}
	if err := reader.Close(); err != nil {
		return err
	}
	if err := file.Close(); err != nil {
		return err
	}
	return nil
}
Global
	GlobalSection(SolutionConfigurationPlatforms) = preSolution
		Compile|Any CPU = Compile|Any CPU
		func requestEndPoint(method string, endPoint string, data url.Values, auth string) (*http.Response, error) {
	link := EndPoint + endPoint + "?" + data.Encode()
	req, _ := http.NewRequest(method, link, nil)
	req.Header = getAuthHeader(auth)
	client := &http.Client{}
	resp, err := client.Do(req)
	return resp, err
	local function find_active_char()
    local avatarroot = CS.UnityEngine.GameObject.Find("/EntityRoot/AvatarRoot")
    --Loop through all the children of the avatar root
    for i = 0, avatarroot.transform.childCount - 1 do
        local child = avatarroot.transform:GetChild(i)
        --If the child is active, return it
        if child.gameObject.activeInHierarchy then
            return child.gameObject
        end
    end
end

local function find_body(avatar)
    for i = 0, avatar.transform.childCount - 1 do
        local transform = avatar.transform:GetChild(i)

        if transform.name == "OffsetDummy" then
            for j = 0, transform.childCount - 1 do
                local child = transform:GetChild(j)

                for k = 0, child.transform.childCount - 1 do
                    local body = child.transform:GetChild(k)

                    if body.name == "Body" then
                        return body.gameObject
                    end
                end
            end
        end
    end
end

local function booba2()
    local avatar = find_active_char()
    CS.MoleMole.ActorUtils.ShowMessage(avatar.name)
    
    if avatar then
        local body = find_body(avatar)

        local bones = body.transform:GetComponent(typeof(CS.UnityEngine.SkinnedMeshRenderer)).bones
        local booba = nil

        --Iterate over bones to find boobs
        for i = 0, bones.Length - 1 do
            local bone = bones[i]
            if bone.name == "Booba" then  --We already made a boobs container earlier
                booba = bone
                break
            end
        end

        if booba == nil then
            --We need to make a boobs container
            local booba_r = nil
            local booba_l = nil
            for i = 0, bones.Length - 1 do
                local bone = bones[i]
                if bone.name == "+Breast L A01" then
                    booba_l = bone
                elseif bone.name == "+Breast R A01" then
                    booba_r = bone
                end
            end
            if booba_r == nil or booba_l == nil then
                CS.MoleMole.ActorUtils.ShowMessage("404 boob not found")
                return
            end
            local boob_parent = booba_l.transform.parent  --Should be <avatar>/Bip001/Bip001 Pelvis/Bip001 Spine/Bip001 Spine1/Bip001 Spine2/

            --Make new boobs container
            booba = CS.UnityEngine.GameObject("Booba")
            booba.transform.parent = boob_parent.transform
            booba.transform.localScale = CS.UnityEngine.Vector3(-1, -1, -1)
            booba.transform.rotation = boob_parent.transform.rotation
            booba.transform.position = boob_parent.transform.position

            --Reparent boob bones to this container
            booba_l.transform.parent = booba.transform
            booba_r.transform.parent = booba.transform
        end

        local scaling_mul = 2
        booba.transform.localScale = CS.UnityEngine.Vector3(scaling_mul, scaling_mul, scaling_mul)
    end
end

local function onError(error)
    CS.UnityEngine.GameObject.Find("/BetaWatermarkCanvas(Clone)/Panel/TxtUID"):GetComponent("Text").text = tostring(error)
	CS.MoleMole.ActorUtils.ShowMessage(tostring(error))
end

xpcall(booba2, onError)
